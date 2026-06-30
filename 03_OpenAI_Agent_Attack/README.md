# 03_OpenAI_Agent_Attack

这个目录用于整理 OpenAI Agent Attack 比赛项目。

## 项目定位

这是一个围绕 Agent 攻击、安全评测、Prompt 搜索、Prompt 变异和评测 Harness 的比赛项目。

核心问题不是单纯写一个 prompt，而是构建一个可以持续搜索、评估、迭代攻击候选的系统。

## 当前文档

```text
03_OpenAI_Agent_Attack/
├── README.md
├── Attack_Framework.md
├── Search_Strategy.md
├── Vector_Search_and_Mutation.md
└── Harness_and_Evaluation.md
```

## 核心思路

```text
Seed Prompt
  ↓
Mutation
  ↓
Evaluation
  ↓
Selection
  ↓
Further Mutation
  ↓
High-scoring Prompt
```

## 已讨论重点

1. 初始模板很重要。
2. 变异质量决定最终上限。
3. 搜索策略不应该只是随机搜索。
4. 可以把 prompt 放到向量空间中做局部搜索。
5. 成功 prompt 附近可能存在更多有效 prompt。
6. Harness / Rubric 是判断攻击是否成功的关键。

