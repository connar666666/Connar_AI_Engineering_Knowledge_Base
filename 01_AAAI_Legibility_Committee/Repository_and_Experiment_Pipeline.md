# 仓库结构与实验管线整理

## 1. 需要重点阅读的仓库路径

当前对话中已经明确，AAAI 项目重点关注：

```text
Multi_Agent_Legibility_Committe/
└── src_lbf_rule/
```

实验入口脚本：

```text
tools/run_lbf_rule_discover_shaping.sh
```

## 2. 为什么入口脚本重要

科研代码中，入口脚本通常比单个函数更重要。因为它定义了：

1. 使用哪个环境。
2. 加载哪些配置。
3. 训练从哪里开始。
4. 是否先做 rule discovery。
5. 是否再做 reward shaping。
6. 实验结果输出到哪里。
7. 是否有多组 seed 或 ablation。

因此，理解 `run_lbf_rule_discover_shaping.sh` 是理解整个实验流程的第一步。

## 3. 建议阅读顺序

```text
run_lbf_rule_discover_shaping.sh
  ↓
参数解析与配置文件
  ↓
环境初始化
  ↓
规则发现模块
  ↓
奖励塑形模块
  ↓
训练主循环
  ↓
日志与输出文件
  ↓
论文图表生成逻辑
```

## 4. 需要整理出的实验管线

最终应该沉淀成一张清晰图：

```text
Config
  ↓
Environment Setup
  ↓
Initial Policy / Trajectory Collection
  ↓
Rule Discovery
  ↓
Shaping Reward Construction
  ↓
Training / Fine-tuning
  ↓
Evaluation
  ↓
Paper Figures / Tables
```

## 5. 后续代码阅读任务

- [ ] 找到所有 shell 脚本入口。
- [ ] 找到 Python 主入口。
- [ ] 找到配置文件。
- [ ] 找到环境定义。
- [ ] 找到 rule discovery 相关类和函数。
- [ ] 找到 reward shaping 相关类和函数。
- [ ] 找到实验输出目录。
- [ ] 找到论文图表生成脚本。

