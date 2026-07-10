# AAAI 可读性项目对话恢复状态

## 项目目标
研究多智能体环境中通过 Rule Discovery 和 Reward Shaping 提升行为可读性，并把最新代码与实验准确写入论文。

## 对话历史关键点
- **用户提出**：应从 `src_lbf_rule/tools/run_lbf_rule_discover_shaping.sh` 入口理解完整实验顺序。
- **用户目标**：根据最新代码和工作修改论文，而不是只补抽象背景。
- **助手补充**：需要建立代码模块、实验产物与 Method、Experiment、Ablation、Discussion 的映射。
- **共同结论**：先确认代码事实与实验产物，再写论文叙事；不能从概念推测实际公式和流程。

## 当前共同结论
阅读顺序为：

```text
Shell 入口
→ Python 主入口
→ Config / Environment
→ Trajectory Collection
→ Rule Discovery
→ Rule Filtering / Scoring
→ Reward Shaping
→ Training
→ Evaluation
→ Figures / Tables
```

论文需要同时说明任务性能和可读性，并区分实现事实、实验结论和叙事解释。

## 待代码验证
- 规则的表示方式。
- 候选规则如何生成、评分和筛选。
- shaping reward 的真实公式、权重和触发位置。
- 训练是否分阶段。
- 日志、Checkpoint、规则文件和图表输出路径。

## 待实验验证
- 可读性是否真正提升。
- 是否牺牲任务性能。
- 不同 seed 的稳定性。
- 不同环境规模的泛化。
- 规则阈值和 shaping weight 的消融结果。

## 知识库加载清单

```text
01_AAAI_Legibility_Committee/README.md
01_AAAI_Legibility_Committee/Repository_and_Experiment_Pipeline.md
01_AAAI_Legibility_Committee/LBF_Rule_Discovery_and_Reward_Shaping.md
01_AAAI_Legibility_Committee/Research_and_Paper_Plan.md
01_AAAI_Legibility_Committee/2026_07_Code_to_Paper_and_Experiment_Update.md
```

## 恢复后继续位置
读取真实仓库 `src_lbf_rule` 和入口脚本，建立“文件—函数—输入—输出—论文段落—实验产物”映射表。