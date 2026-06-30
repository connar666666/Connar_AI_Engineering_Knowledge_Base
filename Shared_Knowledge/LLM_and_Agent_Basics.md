# Shared Knowledge：LLM 与 Agent 基础

## 1. LLM 训练阶段

大模型训练通常包括：

```text
Pretraining
  ↓
Finetuning
  ↓
Post-training / RLHF / GRPO
```

## 2. Pretraining

预训练让模型学习语言、知识和基本模式。它通常使用大量文本数据，通过预测下一个 token 学习通用能力。

## 3. Finetuning

微调是在已有模型基础上，用更小、更专门的数据集让模型适配特定任务或风格。

## 4. Post-training / RL

后训练用于让模型更符合人类偏好、任务目标和安全要求。

RLHF、GRPO 等方法都属于这个阶段可能使用的技术。

## 5. 为什么后训练难

后训练难在：

- 评价标准复杂。
- 奖励信号难设计。
- 模型可能 reward hacking。
- 需要大量高质量偏好数据。
- 稳定性和泛化都难控制。

## 6. Embedding 的目的

Embedding 的目的是把文本、图片等内容变成向量，使系统可以计算语义相似度。

它常用于：

- 搜索。
- 推荐。
- RAG。
- 聚类。
- Prompt 相似度分析。

## 7. RAG

RAG 是 Retrieval-Augmented Generation，即检索增强生成。

它的核心流程是：

```text
用户问题
  ↓
检索相关资料
  ↓
把资料放入上下文
  ↓
模型生成答案
```

## 8. Agent

Agent 不只是聊天模型，而是能够：

- 理解目标。
- 调用工具。
- 规划步骤。
- 执行任务。
- 观察结果。
- 调整策略。

