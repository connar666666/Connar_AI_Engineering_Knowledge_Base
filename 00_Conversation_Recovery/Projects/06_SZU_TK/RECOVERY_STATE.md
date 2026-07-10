# SZU_TK / Transfer Agent 对话恢复状态

## 项目目标
让用户通过自然语言迁移、替换和重组视频或时间线内容，并通过前端和 Remotion 预览结果。

## 对话历史关键点
- **用户提出**：当前 Agent View 可能逐轮累积历史时间线 JSON。
- **用户提出**：最初保留 Snapshot 是为了 KV Cache 命中，但多个历史状态同时进入上下文会造成膨胀和幻觉。
- **用户提出**：考虑滑动窗口、压缩和删除旧上下文。
- **助手补充**：Event Log、Snapshot、Current State、LLM Working Context 应分层。
- **共同结论**：Snapshot 用于恢复，不应默认进入 Prompt；模型只看唯一当前状态、近期操作、有效约束摘要和按需检索历史。

## 当前共同结论
- Agent 应输出结构化 operations，而不是整份新 JSON。
- 后端负责版本、权限、ID 和操作合法性校验。
- Domain State、UI State、Agent Context、Remotion Props 必须分离。
- KV Cache 适合稳定系统前缀，不应以保留过期状态为代价。

## 待验证
- 真实代码中 Agent View 的构造和累积方式。
- Snapshot、上下文拼接、LLM 调用的文件路径和调用链。
- 四个前端模板的 Mock 数据位置。
- operation-based 输出迁移成本。
- token、延迟、缓存命中、旧元素引用率和多轮成功率。

## 知识库加载清单

```text
06_SZU_TK/README.md
06_SZU_TK/Backend_Architecture_and_Data_Flow.md
06_SZU_TK/Transfer_Agent_Context_and_Snapshot_Design.md
06_SZU_TK/Frontend_Remotion_and_Template_Refactor.md
```

涉及面试表达时再读取：

```text
08_Internship/Interview_Review_and_Project_Storytelling.md
```

## 恢复后继续位置
读取用户指定分支的 `apps/web` 和相关后端代码，先还原一轮对话的完整数据流，再验证历史状态累积问题。