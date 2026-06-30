# 01_AAAI_Legibility_Committee

这个目录用于整理 AAAI 可读性项目相关内容。

## 项目定位

AAAI 可读性项目是一个偏科研方向的项目，核心关注：

- Multi-Agent Reinforcement Learning
- Legibility / 可读性
- Rule Discovery / 规则发现
- Reward Shaping / 奖励塑形
- Explainable MARL
- 论文修改与实验补充

之前讨论中反复提到的仓库是：

```text
Multi_Agent_Legibility_Committe
```

重点目录：

```text
src_lbf_rule
```

重点入口脚本：

```text
tools/run_lbf_rule_discover_shaping.sh
```

这个脚本应被视为理解整个实验启动顺序、规则发现流程和 reward shaping 实验管线的入口。

## 当前文档

```text
01_AAAI_Legibility_Committee/
├── README.md
├── Research_and_Paper_Plan.md
├── LBF_Rule_Discovery_and_Reward_Shaping.md
└── Repository_and_Experiment_Pipeline.md
```

## 后续需要继续补充

1. 读取 GitHub 仓库中的 `src_lbf_rule` 目录。
2. 梳理 `run_lbf_rule_discover_shaping.sh` 的实际执行顺序。
3. 把代码逻辑和论文新增内容对应起来。
4. 根据最新实验结果更新论文的 Method、Experiment、Ablation 和 Discussion 部分。

