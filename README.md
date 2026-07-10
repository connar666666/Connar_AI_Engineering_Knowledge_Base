# Connar_AI_Engineering_Knowledge_Base

> 这是从长期对话、项目讨论和工程实践中整理出来的个人 AI 工程知识库。  
> 仓库不是逐字聊天备份，而是把已经形成的架构理解、设计结论、学习笔记、排障经验和待验证问题按项目沉淀为 Markdown 文档。

## 2026 年 7 月增量

本次增量以 2026-07-01 至 2026-07-10 的新增讨论为主。

入口文档：

```text
INCREMENTAL_UPDATE_2026_07.md
```

新增核心目录：

```text
05_Email_MCP/
06_SZU_TK/
07_Hand_Coding/
08_Internship/
```

## 当前项目结构

```text
Connar_AI_Engineering_Knowledge_Base/
├── README.md
├── INCREMENTAL_UPDATE_2026_07.md
├── UPLOAD_TO_GITHUB.md
├── 01_AAAI_Legibility_Committee/
├── 02_ComfyUI_Bench/
├── 03_OpenAI_Agent_Attack/
├── 04_AgentOS_OSAgent/
├── 05_Email_MCP/
├── 06_SZU_TK/
├── 07_Hand_Coding/
├── 08_Internship/
├── Shared_Knowledge/
└── Engineering/
```

## 核心项目

### 01_AAAI_Legibility_Committee

用于整理 AAAI 可读性项目，包括：

- LBF。
- Rule Discovery。
- Reward Shaping。
- 实验入口和执行管线。
- 从代码实现到 Method、Experiment、Ablation、Discussion 的论文映射。

### 02_ComfyUI_Bench

用于整理 ComfyUI / WorkBench / FrameWeave 相关全栈工程内容，包括：

- 数据库设计。
- Project / Canvas 权限和邀请系统。
- FastAPI、Redis、SQL、对象存储和任务队列。
- 前后端设计和部署排障。

> 目录仍沿用初版的 `02_ComfyUI_Bench` 命名，文档内容同时覆盖 WorkBench / FrameWeave 相关讨论。

### 03_OpenAI_Agent_Attack

用于整理 Agent Security 比赛项目，包括：

- Prompt 搜索和变异。
- Replay、Verification 和 Self Reflection。
- 向量空间局部搜索。
- Harness、Rubric 和攻击评测。

### 04_AgentOS_OSAgent

用于整理 AgentOS 与 OS Agent，包括：

- 两个方向的区别。
- Agent 作为操作系统入口的未来形态。
- 记忆、调度、权限、工具和多 Agent 协作。

### 05_Email_MCP

用于整理集团内部 OA Agent 的 Email MCP，包括：

- 企业邮件业务场景。
- SMTP / IMAP / POP3。
- 邮件线程和增量同步。
- 第三方授权、SSO、账号映射和权限。
- MCP Tool List、Skills 和测试。
- 邮箱服务器能力确认清单。

### 06_SZU_TK

用于整理 SZU_TK、Transfer Agent 和 Remotion 相关架构，包括：

- 前后端与 Agent 数据流。
- Agent View、Snapshot 和上下文管理。
- KV Cache、滑动窗口和历史检索。
- 时间线操作、版本控制和渲染 Adapter。
- 前端模板与胶水代码重构。

### 07_Hand_Coding

用于整理算法、数据结构和 Python 工程基础，包括：

- Quickselect、回文和链表。
- LRU Cache + TTL。
- 标准输入输出和测试。
- 多线程、多进程、GIL、锁和 Queue。
- `asyncio`、Task、Future 和 Promise。

### 08_Internship

用于整理实习项目表达和面试复盘，包括：

- 面试问题的结构化复盘。
- Transfer Agent 项目讲解。
- 上下文、KV Cache 和幻觉的工程回答。
- Test、CI、Lint。
- FastAPI、Redis、SQL、Docker 和 HTTP 数据流。

## Shared_Knowledge

保存跨项目复用的基础知识，例如：

- 后端与数据库基础。
- LLM、RAG、Embedding 和 Agent。
- MCP Client、MCP Server 与 Agent Loop。
- Email 和企业系统集成。

## Engineering

保存开发环境、工具和排障经验，例如：

- Git、GitHub 和 PR。
- Codex、Claude Code 和插件。
- WSL、Node/npm 和代理。
- Docker 镜像、仓库和 Compose。
- LaTeX 工程排版。
- 开发设备选择。

## 文档可信度

文档中尽量区分：

- **已确认**：由代码、命令输出或明确需求确认。
- **设计结论**：讨论中已经形成的架构选择。
- **待代码验证**：需要继续读取仓库实现。
- **待实验验证**：需要通过实验或 Benchmark 确认。

## 后续维护方式

每次新增成熟讨论时：

1. 确认属于哪个项目目录。
2. 优先新增或更新主题文档，而不是保存聊天全文。
3. 写清背景、数据流、设计取舍和后续验证项。
4. 通过独立分支和 PR 审阅增量内容。
5. 合并后在增量索引中记录日期和范围。