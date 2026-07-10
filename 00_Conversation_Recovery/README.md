# 00_Conversation_Recovery

这个目录用于帮助新的 GPT 工作空间恢复长期对话状态。

它和仓库中的普通知识文档职责不同：

```text
项目知识库
= 面向人类阅读的完整技术总结、架构设计和学习笔记

Conversation Recovery
= 面向新助手的项目状态、思路来源、共同决策和未完成问题
```

## 1. 核心结构

恢复状态必须按项目隔离，不能把不同项目的对话状态写进同一份长文档。

```text
00_Conversation_Recovery/
├── README.md
├── NEW_WORKSPACE_BOOTSTRAP.md
├── USER_CONTEXT_AND_COLLABORATION_STYLE.md
├── PROJECT_RECOVERY_INDEX.md
└── Projects/
    ├── 01_AAAI_Legibility_Committee/RECOVERY_STATE.md
    ├── 02_ComfyUI_Bench/RECOVERY_STATE.md
    ├── 03_OpenAI_Agent_Attack/RECOVERY_STATE.md
    ├── 04_AgentOS_OSAgent/RECOVERY_STATE.md
    ├── 05_Email_MCP/RECOVERY_STATE.md
    ├── 06_SZU_TK/RECOVERY_STATE.md
    ├── 07_Hand_Coding/RECOVERY_STATE.md
    ├── 08_Internship/RECOVERY_STATE.md
    └── Engineering/RECOVERY_STATE.md
```

## 2. 恢复机制

恢复文件本身不复制完整技术知识，而是告诉新助手：

- 这个项目为什么开始。
- 用户提出过哪些核心思路。
- 助手补充了哪些设计。
- 当前共同结论是什么。
- 哪些事项仍未完成。
- 应按什么顺序读取仓库中该项目的知识文档。

因此，恢复过程不是只读 `RECOVERY_STATE.md`，而是：

```text
项目恢复状态
  ↓
读取其中列出的项目知识库文件
  ↓
必要时读取 Shared_Knowledge / Engineering 依赖
  ↓
如果问题涉及当前实现，再检查真实代码仓库
```

## 3. 新工作空间加载顺序

```text
1. NEW_WORKSPACE_BOOTSTRAP.md
2. USER_CONTEXT_AND_COLLABORATION_STYLE.md
3. PROJECT_RECOVERY_INDEX.md
4. 只选择当前正在讨论的一个项目
5. 读取该项目的 Projects/.../RECOVERY_STATE.md
6. 按恢复文件中的“知识库加载清单”读取对应项目文档
7. 需要时再检查真实代码或最新外部资料
```

不要一次把所有项目恢复状态和整个知识库全部放进上下文。

## 4. 每个项目恢复文件的固定结构

```text
项目身份
当前目标
对话历史关键转折
用户提出的思路
助手补充的思路
共同结论
已确认事实
待代码 / 实验验证
未完成事项
知识库加载清单
恢复后应从哪里继续
```

## 5. 信息来源标记

- **用户提出**：由 FangCong 主动提出的判断、设想、问题或优化方向。
- **助手补充**：助手提供的解释、架构方案、风险分析或实现建议。
- **共同结论**：经过讨论后已经对齐的方向。
- **已确认事实**：由代码、仓库、命令输出、文件或实际测试确认。
- **待代码验证**：需要读取真实实现才能确认。
- **待实验验证**：需要实验、指标或 Benchmark 才能确认。
- **已修正理解**：早期理解后来被澄清或推翻。

## 6. 去重原则

恢复层只保存对继续对话有价值的状态，不重复技术正文。

例如：

- IMAP 原理保存在 `05_Email_MCP/`。
- Email MCP 恢复文件只记录为什么关注线程能力、形成了什么设计、下一步要验证什么，以及应该读取哪些 Email MCP 文档。

## 7. 更新规则

只在以下情况更新项目恢复文件：

- 项目目标变化。
- 用户提出新的核心架构设想。
- 原有共同结论被推翻。
- 代码验证或证伪了历史推测。
- 一个长期开放问题已经闭环。
- 出现新的高优先级下一步。

普通技术知识只更新项目知识库，不重复写入恢复层。

## 8. 隐私边界

仓库当前为公开仓库，不得保存：

- 密码、Token、Cookie、授权码。
- 企业内部邮件正文。
- 未公开的个人信息。
- 私有服务器配置。
- 不适合公开的源代码或业务数据。

可以保存抽象后的架构、问题、设计和经验。