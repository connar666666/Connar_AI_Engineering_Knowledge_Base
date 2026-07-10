# Transfer Agent：上下文、Snapshot 与 KV Cache 设计

## 1. 当前核心矛盾

讨论中识别出的核心问题不是“有没有保存历史”，而是把两件事混在了一起：

```text
为了恢复、撤销和审计而保存历史状态
≠
为了让 LLM 工作而把全部历史状态放进上下文
```

如果每轮 Agent View 都包含此前所有时间线 JSON 或 Snapshot，结果会是：

- 上下文长度持续增长。
- 同一对象的多个旧版本同时出现。
- 模型难以判断哪个状态是当前真相。
- token 成本和延迟持续增加。
- KV Cache 即使有命中，也缓存了大量无效历史。
- 模型可能引用已经被删除或覆盖的旧元素，产生幻觉。

## 2. Snapshot 应该解决什么

Snapshot 适合用于：

- 恢复某个版本。
- 撤销和重做。
- 事故排查。
- 审计。
- 快速加载，避免从第一个事件开始重放。

Snapshot 不应该默认逐个加入 LLM Prompt。

## 3. 推荐的三层状态模型

### 3.1 Event Log

记录每次结构化修改：

```text
operation_id
project_id
base_version
new_version
actor
operation_type
payload
created_at
```

Event Log 是可重放的事实历史。

### 3.2 Checkpoint / Snapshot

每隔固定版本或达到一定事件量，保存一次完整规范化状态。

```text
snapshot(version=40)
+ events(41, 42, 43)
= current_state(version=43)
```

### 3.3 LLM Working Context

只包含：

- 当前状态的必要部分。
- 最近若干轮操作。
- 对长期历史的压缩摘要。
- 当前任务相关的检索结果。

三层数据可以共享来源，但用途不同。

## 4. 滑动窗口不是简单删消息

只保留最近 N 轮可以控制长度，但容易丢失长期约束。因此更合理的是：

```text
稳定系统前缀
+ 当前规范化状态
+ 最近操作窗口
+ 长期约束摘要
+ 按任务检索的历史事件
```

### 最近操作窗口

例如保留最近 5 到 10 次用户意图和 Agent 操作。

### 长期约束摘要

保存仍然有效的约束：

- 用户希望保持的人物风格。
- 不允许修改的片段。
- 视频时长上限。
- 已确认的设计偏好。
- 未完成任务。

摘要必须可以更新和失效，不能无限追加。

## 5. 当前状态必须只有一个权威版本

Prompt 中应明确：

```text
Current State Version: 43
```

模型不需要同时看到 version 1 到 42 的完整状态。如果需要理解某次变化，应检索相应 diff 或事件。

推荐传入：

```json
{
  "state_version": 43,
  "current_state": {...},
  "recent_operations": [...],
  "active_constraints": [...],
  "retrieved_history": [...]
}
```

## 6. 只传局部状态

对于大型时间线，应根据任务选择局部上下文。

例如用户说：

```text
把开头 10 秒换成产品介绍，并保留后面的节奏。
```

Agent 主要需要：

- 0 到 10 秒附近的 clips。
- 与衔接相关的相邻片段。
- 全局时长和节奏约束。
- 可用产品素材。

不一定需要每个字幕节点、每个历史版本和所有未引用资产。

## 7. KV Cache 的正确理解

KV Cache 能减少相同前缀的重复计算，但它不是保留无限历史的理由。

有利于缓存的内容：

- 稳定 System Prompt。
- 固定 Tool Schema。
- 稳定的领域模型说明。

不应为了缓存而保持不变的内容：

- 已经过期的时间线状态。
- 所有历史 Snapshot。
- 大量重复 JSON。

一个较好的结构是：

```text
固定前缀：系统规则 + Tool Schema
变化后缀：当前状态 + 最近交互 + 检索历史
```

这样既有较高前缀复用率，也不会让旧状态污染当前推理。

## 8. 上下文压缩策略

### 结构化 Diff

保存：

```text
added
removed
updated
moved
```

而不是保存两份完整状态。

### 语义摘要

把旧对话压缩为：

```text
用户长期偏好
已经确认的决策
仍未完成的问题
不再有效的约束
```

### 检索历史

当用户引用过去内容时，再按：

- 元素 ID。
- 版本号。
- 操作类型。
- 用户关键词。
- 时间范围。

检索 Event Log 或 Snapshot。

## 9. 推荐上下文预算

可以建立明确预算，而不是让上下文自然增长：

```text
System + Tools        固定预算
Current State         最大预算
Recent Conversation   最大轮数
Active Constraints    最大条数
Retrieved History     Top-K
Reserved Output       必须预留
```

超过预算时，优先：

1. 删除无关资产细节。
2. 把旧操作转换为摘要。
3. 只保留局部时间线。
4. 减少检索历史数量。
5. 绝不删除当前任务的关键约束。

## 10. 幻觉防护

Agent 输出中的每个目标 ID 都必须在当前版本验证。

后端校验：

- 目标是否存在。
- 类型是否匹配。
- base_version 是否过期。
- 操作是否越权。
- 是否违反时间线约束。

如果操作引用旧版本元素，应返回明确错误并要求 Agent 重新规划，而不是自动猜测新目标。

## 11. 评估指标

优化前后应比较：

- 每轮输入 token。
- Prompt 前缀缓存命中率。
- 首 token 延迟和总延迟。
- Agent 操作成功率。
- 旧元素引用错误率。
- 多轮任务完成率。
- 摘要丢失约束的比例。
- 单会话成本。

## 12. 推荐迁移步骤

### 阶段一

- 停止把全部 Snapshot 拼入 Prompt。
- 明确 current version。
- 只传当前状态和最近交互。

### 阶段二

- Agent 从生成完整 JSON 改为生成 operations。
- 增加 version conflict 和 ID 校验。
- 保存 Event Log。

### 阶段三

- 建立 Checkpoint 策略。
- 增加长期约束摘要。
- 增加历史检索。

### 阶段四

- 根据真实 token、缓存和成功率数据调优窗口大小。

## 13. 最终结论

保存完整历史是数据层的正确选择；把完整历史交给模型通常是上下文层的错误选择。

推荐架构：

```text
Event Log + Periodic Snapshot
            ↓
       Current State
            ↓
Task-aware Context Builder
            ↓
       Transfer Agent
```