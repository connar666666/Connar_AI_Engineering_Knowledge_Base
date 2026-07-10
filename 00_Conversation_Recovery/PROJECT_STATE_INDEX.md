# 项目状态索引

本文件只保存继续对话所需的核心状态，不重复项目目录中的完整技术内容。

## 1. Email MCP

### 项目目标

在集团 OA Agent 中接入企业邮箱，使用户能够查询、搜索、收发、回复、转发、追踪历史邮件并聚合邮件会话。

### 讨论来源

- **用户提出**：企业内部仍有大量非固定流程依赖邮件，需求不能只围绕基础收发，而要覆盖跨部门、供应商、合同、附件、历史追踪和会话聚合。
- **用户提出**：OA Agent 已有企业微信 / SSO 身份，希望减少用户逐个配置邮箱授权的成本。
- **助手补充**：应区分 OA 身份、邮箱账号和邮箱访问凭据；MCP 不应独自承担企业权限体系。
- **共同结论**：OA Agent 管身份、授权、策略和确认；Email MCP 管协议适配和工具执行；邮箱服务器管邮箱数据、服务端能力和限制。

### 当前状态

- 已确认 IMAP / SMTP 与第三方客户端专用密码可用。
- 已形成服务器能力确认清单。
- 已讨论邮件线程不能假设存在统一 thread id，应优先利用 `Message-ID`、`In-Reply-To` 和 `References`。
- 已讨论 Tool、Skill、会话聚合、测试和安全边界。

### 尚未闭环

- 当前 `Email_MCP/emails/` 真实代码中的 Tool 注册、Service 分层、测试覆盖和 Skills 质量仍需继续代码验证。
- 需要比较自研实现和 fork 的 `mcp-email-server` 的工具差异。
- 需要邮箱服务器团队回复 IMAP 扩展、并发、统一授权和原生会话能力。

### 继续阅读

```text
05_Email_MCP/
Shared_Knowledge/MCP_Email_and_Agent_Integration.md
```

---

## 2. SZU_TK / Transfer Agent

### 项目目标

让用户通过自然语言迁移、替换和重组视频或时间线内容，并通过前端与 Remotion 预览最终结果。

### 讨论来源

- **用户提出**：当前 Agent View 可能在每轮累积历史时间线 JSON，导致上下文越来越长。
- **用户提出**：原本希望保留过去 Snapshot 来提高 KV Cache 命中，但意识到这会把多个历史状态同时交给模型，引发幻觉。
- **用户提出**：考虑滑动窗口、压缩和删除旧上下文。
- **助手补充**：应把 Event Log、Snapshot、当前状态和 LLM Working Context 分层；Snapshot 用于恢复，不应默认进入 Prompt。
- **共同结论**：模型只看到唯一当前状态、近期操作、有效约束摘要和按需检索历史；Agent 最好输出结构化 operations，而不是复制完整 JSON。

### 当前状态

已经形成上下文重构方向：

```text
Event Log + Periodic Snapshot
            ↓
       Current State
            ↓
Task-aware Context Builder
            ↓
       Transfer Agent
```

同时形成 Remotion 和前端分层方向：

```text
Domain State
→ Selector / Adapter
→ Remotion Props
→ Preview / Render
```

### 尚未闭环

- 需要继续读取真实分支，确认 Agent View JSON 是否真的逐轮累积。
- 需要找到具体上下文构造文件、状态保存文件和前端调用链。
- 需要确认四个模板的 Mock 数据位置和实际引用。
- 需要量化 token、缓存命中、旧状态引用错误率和多轮成功率。

### 继续阅读

```text
06_SZU_TK/
08_Internship/Interview_Review_and_Project_Storytelling.md
```

---

## 3. AAAI 可读性项目

### 项目目标

研究多智能体环境中通过 Rule Discovery 和 Reward Shaping 提升行为可读性，并将最新代码与实验写入论文。

### 讨论来源

- **用户提出**：应从 `src_lbf_rule/tools/run_lbf_rule_discover_shaping.sh` 这个实验入口理解完整执行过程，再修改论文。
- **助手补充**：需要建立代码模块到 Method、Experiment、Ablation 和 Discussion 的逐项映射。
- **共同结论**：先确认代码事实和实验产物，再写论文叙事；不能从概念推测实际奖励公式和实验流程。

### 当前状态

已经确定阅读链：

```text
Shell 入口
→ Python 主入口
→ Config / Environment
→ Trajectory Collection
→ Rule Discovery
→ Reward Shaping
→ Training
→ Evaluation
→ Figure / Table
```

### 尚未闭环

- 需要完整读取真实代码。
- 需要确认规则表示、评分、筛选和保存方式。
- 需要确认 shaping reward 公式、权重和触发逻辑。
- 需要整理实验矩阵、结果和论文证据表。

### 继续阅读

```text
01_AAAI_Legibility_Committee/
```

---

## 4. OpenAI Agent Attack

### 项目目标

构建系统化的 Agent 攻击搜索框架，而不是只写单个攻击 Prompt。

### 讨论来源

- **用户提出**：利用成功 Prompt 的 embedding 邻域继续搜索更有效攻击。
- **用户提出**：成功攻击后保存样本，并进行变异、Replay 和再次验证。
- **用户提出**：搜索策略应利用 hit 结果变得更有方向，而不是重复随机模板。
- **助手补充**：需要分离 Search、Mutation、Replay、Verification、Dedup 和 Harness。
- **共同结论**：成功与失败样本都应保存；搜索过程必须可复现，Harness 是整个系统的核心。

### 当前状态

已形成 LLM Search + Mutation + Replay + Self Reflection 的升级框架，以及向量空间局部搜索方向。

### 尚未闭环

- 需要继续结合比赛代码和最新 notebook 验证实现。
- 需要明确攻击成功判定、候选重复规则和 replay 策略。
- 需要形成完整可执行升级设计与实验对比。

### 继续阅读

```text
03_OpenAI_Agent_Attack/
```

---

## 5. ComfyUI WorkBench / FrameWeave

### 项目目标

构建围绕项目、Canvas、资产、生成任务、权限和视频工作流的全栈平台。

### 讨论来源

- **用户提出**：Project 权限可以向下复用给所有 Canvas，但 Canvas 权限不能反向扩大到 Project。
- **用户提出**：Project 和 Canvas 都需要邀请用户，首页要有邀请通知。
- **助手补充**：应使用独立的成员表与邀请表，保留 pending、accepted、declined 等状态和审计信息。
- **共同结论**：Project 与 Canvas 权限是非对称继承；邀请记录不能只在接受后生成成员关系。

### 当前状态

已讨论数据库 V3、权限、邀请、FastAPI、Redis、SQL、对象存储、Docker 和前端交互。

### 尚未闭环

- 需要结合最新代码确认数据库和前端是否已实现。
- 需要继续完成真实 ER、API、索引和权限 SQL。
- Remotion / 视频一致性相关实现需要结合具体仓库继续验证。

### 继续阅读

```text
02_ComfyUI_Bench/
```

---

## 6. 手撕代码与 Python 工程学习

### 学习目标

为算法、Python 和工程实习面试建立可解释、可手写、可测试的能力。

### 当前状态

已经重点讨论：

- Quickselect。
- 链表反转。
- LRU Cache + TTL。
- 最长回文子串。
- 标准输入输出。
- 线程、进程、线程池、进程池、GIL。
- asyncio、Coroutine、Task、Future、Promise。

### 学习方式结论

- **用户反馈**：一次性给大量零散知识学习效率很低。
- **共同结论**：后续应以真实服务场景逐步讲解，并通过手写代码和输入输出测试巩固。

### 尚未闭环

- 继续补全并发实战案例。
- 对 LRU、链表和算法题继续进行手写训练。
- 为面试准备稳定的解释模板和测试方法。

### 继续阅读

```text
07_Hand_Coding/
```

---

## 7. 实习与面试

### 目标

把真实项目经验转化为能在面试中讲清楚的业务目标、架构、个人贡献、验证和反思。

### 当前状态

已形成标准复盘结构：

```text
问题
→ 原回答
→ 有效部分
→ 缺陷
→ 优化回答
→ 项目证据
→ 补强任务
```

已重点准备 Transfer Agent、全栈数据流、CI / Test / Lint、上下文管理和 Agent 幻觉问题。

### 尚未闭环

- 原始面试逐字稿需要继续逐题整理。
- 项目回答需要补真实文件路径、接口、测试和量化数据。
- 需要准备更多失败案例和修复过程。

### 继续阅读

```text
08_Internship/
```

---

## 8. AgentOS 与 OS Agent

### 核心观点来源

- **用户提出**：如果 AgentOS 成熟，未来用户可能不再关心打开 App、管理账号，而是由 Agent 直接调度系统能力。
- **助手补充**：OS Agent 更像运行在现有系统之上的过渡形态；AgentOS 则要求系统权限、调度、记忆和应用能力为 Agent 原生设计。
- **共同结论**：OS Agent 很可能是走向 AgentOS 的中间阶段，但真正落地取决于安全、权限、可靠性和生态开放。

### 尚未闭环

- 继续跟踪最新研究和产业实现。
- 将 AgentOS 的权限、记忆、调度和应用接口抽象成更完整系统架构。

### 继续阅读

```text
04_AgentOS_OSAgent/
```

---

## 9. 工程环境与工具

### 长期问题

用户同时使用 Mac、Windows 和 WSL，常见问题包括：

- Windows 与 WSL 的 Node/npm 路径混用。
- Codex / Claude CLI 安装和平台依赖。
- 代理、GitHub 和 Docker 网络。
- Docker Desktop 与项目终端环境差异。
- Git 凭据、分支、PR 和提交历史。

### 协作结论

排障时应先确认：

```text
实际执行的是哪个命令
→ 环境变量
→ 运行时版本
→ 网络和代理
→ 项目配置
→ 宿主日志
```

不要在未定位差异前反复重装。

### 继续阅读

```text
Engineering/
```