# OpenAI Agent Attack：向量搜索与 Prompt 变异

## 1. 向量空间思路

我们讨论过一个关键想法：可以把 prompt 映射到 embedding 空间中。

如果一个 prompt 已经成功，那么它附近的 prompt 可能也有较高成功概率。

```text
Successful Prompt
  ↓ embedding
Vector Space
  ↓ nearest neighbors / local perturbation
Candidate Prompts
  ↓ evaluation
```

## 2. 为什么有意义

Prompt 不是随机字符串，而是语义结构。语义相近的 prompt 往往会触发相近的模型行为。

因此，成功样本附近可能存在一片有效区域。

## 3. 变异策略

### 3.1 语义保持变异

保持原意，但改变表达方式。

### 3.2 结构变异

改变 prompt 的结构，例如先给背景，再给任务，再给约束。

### 3.3 强化成功片段

保留成功 prompt 中有效片段，并替换其他部分。

### 3.4 局部扰动

围绕成功 prompt 做小幅修改，寻找更高分版本。

## 4. 需要记录的信息

每次变异都应该保存：

```text
parent_prompt
mutation_method
new_prompt
embedding_distance
score
success_or_failure
```

## 5. 风险

1. embedding 相似不等于攻击效果相似。
2. 过度依赖成功样本会陷入局部最优。
3. 变异可能违反比赛重复规则。
4. 需要判断同一 prompt 或高度相似 prompt 是否允许重复攻击。

