# OpenAI Agent Attack 对话恢复状态

## 项目目标
构建可复现、可反馈优化的 Agent 攻击搜索系统，而不是只依赖人工编写单个攻击 Prompt。

## 对话历史关键点
- **用户提出**：利用成功 Prompt 的 embedding 邻域继续搜索更有效攻击。
- **用户提出**：攻击成功后保存样本，继续 Mutation、Replay 和验证。
- **用户提出**：搜索策略应利用 hit 结果变得更有方向，减少重复和随机试探。
- **助手补充**：系统应拆分 Search、Mutation、Replay、Verification、Dedup、Self Reflection 和 Harness。
- **共同结论**：成功与失败样本都要保存；Harness 和成功判定是整个系统的核心。

## 当前共同结论
- 搜索过程必须可复现并记录配置、候选、结果和失败原因。
- 成功样本可作为局部搜索种子，但需要避免局部最优和重复规则。
- Replay 应验证攻击是否稳定，而不是把一次命中直接当成可靠成功。
- 向量相似不等于攻击效果相似，需要实验验证。

## 待验证
- 比赛对单轮、多轮 Prompt 和重复候选的具体规则。
- 当前 notebook 中候选生成、验证和 replay 的真实实现。
- 成功判定标准和 Harness 输出。
- embedding 邻域搜索是否带来稳定提升。
- LLM Mutation 和模板 Mutation 的对比。

## 知识库加载清单

```text
03_OpenAI_Agent_Attack/README.md
03_OpenAI_Agent_Attack/Attack_Framework.md
03_OpenAI_Agent_Attack/Search_Strategy.md
03_OpenAI_Agent_Attack/Vector_Search_and_Mutation.md
03_OpenAI_Agent_Attack/Harness_and_Evaluation.md
```

## 恢复后继续位置
读取当前比赛 notebook 和实现代码，先还原一次候选从生成、提交、验证到 Replay 的完整生命周期，再设计升级实验。