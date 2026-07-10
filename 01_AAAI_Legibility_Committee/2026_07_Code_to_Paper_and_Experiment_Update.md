# AAAI 可读性项目：2026 年 7 月代码到论文增量

## 1. 本轮目标

当前阶段不是重新解释 Legibility、Rule Discovery 和 Reward Shaping 的基础概念，而是把最新代码实现、实验执行顺序和论文修改建立一一对应关系。

重点入口：

```text
Multi_Agent_Legibility_Committe/
└── src_lbf_rule/
    └── tools/run_lbf_rule_discover_shaping.sh
```

## 2. 为什么从 Shell 入口开始

一个实验入口脚本通常决定：

- 使用哪个环境和配置。
- 随机种子和实验组。
- 初始策略从哪里加载。
- 先采样、先发现规则还是直接训练。
- 规则文件如何保存和复用。
- Reward Shaping 如何开启。
- 输出目录、日志和 Checkpoint。

因此论文不能只阅读某个算法类，而要先还原完整执行图。

## 3. 推荐代码阅读链路

```text
run_lbf_rule_discover_shaping.sh
  ↓ 命令和参数
Python main entry
  ↓
Config / Environment registration
  ↓
Policy loading or initial training
  ↓
Trajectory collection
  ↓
Rule discovery
  ↓
Rule filtering / scoring
  ↓
Shaping reward construction
  ↓
Training or fine-tuning
  ↓
Evaluation
  ↓
Logs / tables / figures
```

对每一步需要记录：

```text
输入
输出
关键参数
调用文件
产物路径
失败条件
对应论文段落
```

## 4. 建议建立代码—论文映射表

| 代码模块 | 需要确认的问题 | 论文位置 |
|---|---|---|
| Environment | observation、action、team reward 如何定义 | Problem Formulation |
| Trajectory Collector | 采集哪些轨迹、是否筛选成功轨迹 | Data Collection |
| Rule Discovery | 候选规则如何表示和生成 | Method |
| Rule Scoring | support、confidence、performance 如何计算 | Method |
| Reward Shaping | 规则如何转换为额外奖励 | Method |
| Trainer | 原始 reward 与 shaping reward 如何组合 | Training |
| Evaluator | 性能和可读性如何测量 | Experiments |
| Launcher | seed、环境规模、实验组合 | Experimental Setup |

## 5. Rule Discovery 需要回答的具体问题

论文中不能只写“从轨迹中发现规则”，至少需要确认：

1. 规则的最小表示是什么。
2. 条件基于全局状态还是局部 observation。
3. 规则的动作结论是单 Agent 还是联合行为。
4. 候选规则是枚举、搜索、统计还是模型生成。
5. 如何去除重复、冲突和低支持规则。
6. 是否只使用成功轨迹。
7. 规则是否按环境、任务或 Agent 分开保存。
8. 规则文件是否会在训练阶段被重新加载。

## 6. Reward Shaping 需要回答的具体问题

需要从代码确认：

```text
r_total = r_env + lambda * r_rule
```

是否真的是这种形式，以及：

- `lambda` 如何设置。
- 规则满足时给正奖励还是也存在惩罚。
- 奖励是稀疏事件还是每步计算。
- 多条规则同时触发如何合并。
- 是否做归一化或裁剪。
- 是否改变最优策略。
- 是否存在潜在函数形式的 shaping。

不能在未核对代码时直接在论文中写死公式。

## 7. 论文修改顺序

### 第一步：实现事实表

先从代码记录事实，不急于润色：

```text
模块
文件
函数
输入
输出
超参数
产物
```

### 第二步：实验矩阵

建议至少区分：

```text
Baseline
Baseline + Rule Discovery only
Baseline + Shaping
不同规则阈值
不同 shaping weight
不同 seed
不同环境规模
```

### 第三步：论文叙事

建立逻辑链：

```text
任务性能不足以描述策略质量
  ↓
需要可读性目标
  ↓
从行为轨迹提取可解释规则
  ↓
用规则提供训练信号
  ↓
同时评估性能与可读性
```

### 第四步：消融与失败分析

需要回答：

- 没有规则筛选会怎样。
- 只发现规则但不 shaping 是否有效。
- shaping weight 过大是否损害性能。
- 规则在不同 seed 上是否稳定。
- 规则是否泛化到不同地图或 Agent 数量。

## 8. 可读性指标的风险

可读性不能只依赖一个自定义分数。建议同时记录：

- 任务成功率。
- 平均回报。
- 规则覆盖率。
- 规则一致性。
- 观察者目标识别准确率或预测速度。
- 不同 seed 的方差。
- 行为轨迹示例。

如果使用人工评价，需要说明评价流程和一致性；如果使用自动指标，需要解释它与人类可理解性的关系。

## 9. 实验可复现性清单

- Python 和依赖版本。
- 环境版本。
- 训练硬件。
- 随机种子。
- 完整命令。
- 配置文件。
- Checkpoint 来源。
- 规则产物。
- 日志和结果目录。
- 图表生成脚本。

## 10. 可信度标记

本文件当前记录的是阅读与写作框架。

- **已确认**：主要仓库、`src_lbf_rule` 和入口脚本路径来自项目讨论。
- **待代码验证**：具体参数、函数调用、奖励公式和实验组合。
- **待实验验证**：可读性提升、性能 trade-off 和规则泛化结论。

## 11. 最终交付目标

最终应形成三份互相对应的产物：

```text
1. 实验执行流程图
2. 代码—论文映射表
3. 实验结果—论文结论证据表
```

这样论文新增内容才能真正由当前代码和实验支撑，而不是停留在概念补写。