# OpenAI Agent Attack：攻击框架设计

## 1. 项目目标

比赛项目的目标不是简单写出一个攻击 prompt，而是构建一套系统化的攻击搜索框架。

系统应该能够：

- 生成候选 prompt。
- 变异 prompt。
- 评估 prompt 是否成功。
- 保存成功样本。
- 避免重复攻击。
- 根据成功样本继续搜索。

## 2. 基本框架

```text
Prompt Template Library
  ↓
Prompt Generator
  ↓
Mutation Engine
  ↓
Target Agent Execution
  ↓
Harness Evaluation
  ↓
Score / Label
  ↓
Candidate Pool Update
```

## 3. 主要模块

### 3.1 Seed Prompt Library

保存初始模板。模板质量非常重要，因为搜索空间很大，如果初始模板太差，后续变异很难进入有效区域。

### 3.2 Mutation Engine

负责对 prompt 做变异，例如：

- 替换措辞。
- 改变指令顺序。
- 增加上下文。
- 改变角色设定。
- 添加约束或诱导。
- 局部改写成功 prompt。

### 3.3 Evaluator / Harness

负责判断攻击是否成功。

这部分比 prompt 本身更重要，因为如果评估不稳定，搜索系统就会学习错误方向。

### 3.4 Candidate Database

保存：

- 成功 prompt。
- 失败 prompt。
- 分数。
- 变异来源。
- 相似 prompt。
- 是否重复。

## 4. 系统设计重点

1. 搜索过程要可复现。
2. 每次评估要记录日志。
3. 成功样本要结构化保存。
4. 失败样本也有价值，可以避免重复搜索。
5. 变异策略要逐步从随机变成有方向搜索。

