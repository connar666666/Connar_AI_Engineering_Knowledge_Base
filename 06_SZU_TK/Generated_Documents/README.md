# SZU_TK / Transfer Agent：历史生成文档归档

## 权威成品

| 历史交付主题 | 权威文件 | 状态 |
|---|---|---|
| 后端架构、Agent 输入输出和完整数据流 | `../Backend_Architecture_and_Data_Flow.md` | 权威成品 |
| Agent View、Snapshot、KV Cache 与上下文重构 | `../Transfer_Agent_Context_and_Snapshot_Design.md` | 权威成品 |
| 前端、Remotion、模板和胶水代码重构 | `../Frontend_Remotion_and_Template_Refactor.md` | 权威成品 |

## 历史请求来源

用户曾要求：

- 讲清 Transfer Agent 多轮对话的数据流。
- 检查每轮 Agent View 是否累积历史 JSON。
- 解决 Snapshot、KV Cache、上下文膨胀和幻觉冲突。
- 给出滑动窗口、摘要、删除和按需检索方案。
- 检查 Remotion 胶水代码和四个 Mock 模板。
- 为面试准备迁移效果的完整解释。

现有三份文档共同构成当前完整的设计交付。

## 尚未形成代码事实成品

- 对指定分支真实代码的逐文件审计报告。
- Agent View 历史累积的代码证据。
- 四个模板的确切文件位置和引用关系。
- operation-based 重构后的真实 Patch。
- 优化前后 token、缓存、延迟和成功率报告。

这些必须基于最新仓库验证，不能只依据历史设计文档。