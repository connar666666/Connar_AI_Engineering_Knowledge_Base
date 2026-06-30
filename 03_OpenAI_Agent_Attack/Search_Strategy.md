# OpenAI Agent Attack：搜索策略

## 1. 为什么搜索策略重要

在 Agent Attack 比赛中，prompt 空间非常大。只靠人工写 prompt 或随机变异，很难稳定找到高分攻击。

因此，需要把问题看成一个搜索问题。

## 2. 搜索循环

```text
初始化候选 prompt
  ↓
执行攻击
  ↓
评估得分
  ↓
选择高分样本
  ↓
围绕高分样本变异
  ↓
继续搜索
```

## 3. 初始模板的重要性

初始模板决定搜索起点。

如果初始模板完全无效，后续变异可能只是在无效区域附近波动。

因此需要维护多个 seed prompt：

- 直接攻击型。
- 间接诱导型。
- 角色扮演型。
- 多步推理型。
- 任务伪装型。

## 4. Exploitation 与 Exploration

搜索需要平衡：

```text
Exploitation：围绕已成功 prompt 深挖。
Exploration：尝试新的攻击方向。
```

如果只 exploitation，容易陷入局部最优。

如果只 exploration，效率低。

## 5. 成功 prompt 库

成功 prompt 应保存为结构化记录：

```text
prompt_text
score
attack_type
mutation_source
target_behavior
evaluation_result
created_at
```

## 6. 失败 prompt 的价值

失败 prompt 也应该保存，因为它可以帮助：

- 避免重复攻击。
- 分析哪些方向无效。
- 训练或构造失败区域边界。

