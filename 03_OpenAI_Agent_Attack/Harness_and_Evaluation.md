# OpenAI Agent Attack：Harness 与评测机制

## 1. Harness 为什么重要

比赛中真正关键的不只是攻击 prompt，而是如何稳定判断攻击是否成功。

如果评估器不可靠，搜索算法就会被错误信号引导。

## 2. Harness 的基本流程

```text
输入 prompt
  ↓
目标 Agent 执行
  ↓
收集输出
  ↓
根据 rubric 判断
  ↓
返回成功 / 失败 / 分数
  ↓
记录日志
```

## 3. Rubric 的作用

Rubric 是判断输出是否满足攻击目标的标准。

一个好的 Rubric 应该：

- 明确成功条件。
- 明确失败条件。
- 能处理边界情况。
- 尽量减少主观判断。
- 支持自动化评测。

## 4. 为什么这类似 Harness 工程

之前我们讨论过，Rubric 机制本质上类似测试 Agent 能力的 Harness 工程。

原因是：

```text
Agent 输出是否好，不应该只靠感觉判断；
需要有明确标准、输入、输出和评估规则。
```

这和软件测试类似：

```text
Test Case
  ↓
Run Program / Agent
  ↓
Check Output
  ↓
Pass / Fail
```

## 5. 评测记录

每次评测应该保存：

```text
prompt
agent_output
rubric_result
score
failure_reason
timestamp
```

## 6. 后续优化方向

- 更细粒度 Rubric。
- 多阶段评估。
- 自动聚类失败原因。
- 记录不同模型上的攻击迁移性。
- 将成功与失败样本用于改进搜索策略。

