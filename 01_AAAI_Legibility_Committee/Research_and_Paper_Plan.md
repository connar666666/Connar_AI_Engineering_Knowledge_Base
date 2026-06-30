# AAAI 可读性项目：研究背景与论文修改计划

## 1. 项目背景

AAAI 可读性项目关注的是多智能体环境中的行为可读性、规则发现和奖励塑形问题。核心问题不是单纯提高任务分数，而是让智能体行为更容易被观察者、人类或其他智能体理解。

这个方向涉及几个关键词：

```text
Legibility
Multi-Agent Reinforcement Learning
Reward Shaping
Rule Discovery
Explainable MARL
LBF Environment
```

## 2. 为什么需要“可读性”

传统强化学习常常只关注最终 reward 或 task success，但在多智能体系统中，只看任务成功是不够的。因为真实场景中还需要理解：

1. Agent 为什么这样行动。
2. Agent 的行动是否传达了意图。
3. 观察者能否根据行为推断目标。
4. 多 Agent 协作过程中是否存在可解释的协作模式。
5. 学到的策略是否只是黑盒技巧，还是有可总结的规则。

因此，可读性研究的目标不是替代性能优化，而是补充一个新的评价维度：行为是否容易被理解。

## 3. 论文修改的总体思路

论文修改不应该只是把新代码和新实验“补上去”，而应该建立一条清晰逻辑链：

```text
原始问题
  ↓
为什么任务成功不足以说明策略好
  ↓
为什么需要 Legibility
  ↓
如何通过 Rule Discovery 提取行为规则
  ↓
如何通过 Reward Shaping 引导可读行为
  ↓
实验如何证明方法有效
```

## 4. 需要补充的论文内容

### 4.1 Method 部分

需要说明：

- LBF 环境如何定义。
- Agent 之间如何交互。
- Rule Discovery 模块如何工作。
- Shaping Reward 如何构造。
- 可读性指标如何定义。

### 4.2 Experiment 部分

需要补充：

- 实验启动脚本。
- 训练流程。
- Baseline。
- 消融实验。
- 不同规则发现方式的比较。
- Reward shaping 前后的行为变化。

### 4.3 Discussion 部分

需要讨论：

- 可读性和性能之间是否存在 trade-off。
- 规则发现是否稳定。
- 不同环境规模下方法是否泛化。
- 是否存在 reward hacking。
- 可读性指标是否足够可靠。

## 5. 和代码对应的论文写法

目前应该优先阅读并理解：

```text
src_lbf_rule/
tools/run_lbf_rule_discover_shaping.sh
```

论文中的新增内容应该直接对应代码中的模块，而不是抽象描述。

例如：

```text
代码模块：rule discovery
论文内容：规则发现方法与候选规则生成

代码模块：shaping reward
论文内容：如何根据规则构造额外奖励

代码模块：experiment launcher
论文内容：实验配置、训练顺序、输出文件
```

## 6. 后续 TODO

- [ ] 完整读取 `src_lbf_rule`。
- [ ] 画出实验管线图。
- [ ] 根据脚本整理命令参数。
- [ ] 对应论文 Method 部分逐段补充。
- [ ] 整理论文新增图表。

