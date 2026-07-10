# OpenAI Agent Attack：历史生成文档归档

## 权威成品

| 历史交付主题 | 权威文件 | 状态 |
|---|---|---|
| 攻击算法整体框架 | `../Attack_Framework.md` | 权威成品 |
| Prompt 搜索、选择与反馈策略 | `../Search_Strategy.md` | 权威成品 |
| 向量空间搜索与成功样本变异 | `../Vector_Search_and_Mutation.md` | 权威成品 |
| Harness、Rubric 与成功判定 | `../Harness_and_Evaluation.md` | 权威成品 |

## 对应历史交付

上述四份文件共同构成用户曾要求的“完整升级设计文档”，覆盖：

```text
LLM Search
→ Mutation
→ Evaluation
→ Replay
→ Verification
→ Self Reflection
→ Successful Seed Reuse
```

同时保留了用户提出的核心方向：

- 利用成功 Prompt 的 hit 结果继续搜索。
- 在 embedding 邻域搜索相似攻击候选。
- 成功后保存、变异并 Replay。
- 减少重复规则和无方向模板。

## 尚未形成成品

- 根据最新比赛 notebook 逐行核对后的实现修改方案。
- 可运行的状态机代码。
- 向量搜索、LLM Mutation、模板 Mutation 的实验结果对比。

这些仍依赖比赛代码和提交环境，不作为已完成成品归档。