# 06_SZU_TK

这个目录整理 SZU_TK 项目及其 Transfer Agent、前端时间线、Remotion 渲染和上下文管理相关讨论。

## 项目核心问题

项目不是简单调用大模型生成一段结果，而是让 Agent 读取当前编辑状态、理解用户迁移意图、修改时间线或页面结构，并在多轮对话中持续迭代。

```text
用户自然语言要求
  ↓
前端收集当前项目 / 时间线状态
  ↓
Transfer Agent 规划和生成操作
  ↓
后端校验、执行或持久化
  ↓
前端更新状态并进行 Remotion 预览 / 渲染
```

## 本轮重点

- 当前 Agent View 是否在每轮都包含全部历史状态。
- Snapshot 的保存方式与 LLM 上下文的使用方式是否混在一起。
- 为了 KV Cache 命中而保留长前缀，是否反而造成上下文膨胀和幻觉。
- 如何用事件日志、Checkpoint、滑动窗口和摘要替代全量 JSON 累积。
- Remotion 相关胶水代码、模板 Mock 数据和真实分析结果如何分离。

## 当前文档

```text
06_SZU_TK/
├── README.md
├── Backend_Architecture_and_Data_Flow.md
├── Transfer_Agent_Context_and_Snapshot_Design.md
└── Frontend_Remotion_and_Template_Refactor.md
```

## 可信度说明

本目录主要沉淀对话中形成的架构判断。具体文件路径、类名和当前实现状态，需要继续以 SZU_TK 对应分支代码为准。