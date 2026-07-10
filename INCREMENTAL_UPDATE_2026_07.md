# 2026 年 7 月增量知识整理

> 整理范围：以 2026-07-01 之后的新增对话为主，截止 2026-07-10。  
> 整理原则：不直接保存逐句聊天记录，而是沉淀为项目背景、架构结论、数据流、设计决策、问题清单和后续验证项。

## 1. 本次新增目录

```text
05_Email_MCP/
06_SZU_TK/
07_Hand_Coding/
08_Internship/
```

同时增量更新：

```text
01_AAAI_Legibility_Committee/
Shared_Knowledge/
Engineering/
```

## 2. Email MCP

本轮新增讨论集中在企业内部 OA Agent 如何接入集团邮箱，覆盖：

- 企业邮件业务场景与需求边界。
- SMTP、IMAP、POP3 的职责差异。
- IMAP UID、批量拉取、增量同步和 IDLE。
- 邮件线程聚合与 `Message-ID`、`In-Reply-To`、`References`。
- 第三方客户端授权码、统一认证、账号映射与权限控制。
- MCP Server 的工具列表、会话模型、Skills 和 Prompt 组织。
- 自研 Email MCP 与开源 `mcp-email-server` 的能力差异。
- 面向邮箱服务器团队的最终确认问题清单。

## 3. SZU_TK / Transfer Agent

本轮新增讨论集中在 Agent 驱动的视频/时间线迁移系统，覆盖：

- 前后端与 Agent 的完整数据流。
- Transfer Agent 多轮对话的上下文管理。
- Agent View JSON 在每轮累积导致的上下文膨胀。
- 历史 Snapshot、KV Cache 命中率和幻觉风险之间的冲突。
- 滑动窗口、状态摘要、事件日志和 Snapshot 分层保存方案。
- Remotion 胶水代码、模板占位数据和前端模块重构。

## 4. AAAI 可读性项目

本轮继续明确了从科研代码到论文修改的工作路径：

- 以 `src_lbf_rule/tools/run_lbf_rule_discover_shaping.sh` 为实验入口。
- 沿着配置、轨迹采样、规则发现、奖励塑形、训练、评估梳理执行链路。
- 将代码模块逐一映射到论文 Method、Experiment、Ablation 和 Discussion。
- 区分已经由代码确认的事实、需要实验验证的假设和论文叙述层面的解释。

## 5. 手撕代码与 Python 工程基础

本轮新增内容包括：

- Quickselect 的分区逻辑和第 k 大/第 k 小下标转换。
- LRU Cache + TTL 的哈希表与双向链表设计。
- 命令行输入输出、`sys.stdin` 解析与测试主函数。
- 最长回文子串的中心扩展和 Manacher 思路。
- 多线程、多进程、线程池、进程池、GIL、锁、Queue。
- `asyncio`、协程、Task、Future 与 TypeScript Promise 的关系。

## 6. 实习与面试复盘

本轮新增内容包括：

- 将面试记录整理为“问题—原回答—问题点—优化回答”的结构。
- Transfer Agent 项目如何讲清业务目标、技术架构和迁移效果。
- 如何回答上下文管理、KV Cache、Agent 幻觉、前后端协同等追问。
- PR 中 test、CI、lint 的职责和工程价值。
- FastAPI、Redis、SQL、Docker、HTTP 在真实服务中的协作方式。

## 7. 工程与开发环境

本轮增量记录：

- Docker 镜像、镜像仓库和阿里云镜像仓库。
- Docker 镜像是 Docker Engine 全局资源，不属于某个项目目录。
- WSL 中 Windows Node/npm 与 Linux Node/npm 混用问题。
- Codex 插件安装、插件可见但不可调用、重载和排查路径。
- Git commit SHA 不可原地修改，只能通过重写历史产生新的提交 SHA。
- GitHub PR 的 test、CI、lint 检查。
- LaTeX `algorithm` / `algpseudocode` 包冲突和最小化依赖原则。

## 8. 文档可信度标记

本次文档使用以下标记：

- **已确认**：由代码、命令输出或明确需求确认。
- **设计结论**：对话中已经形成的架构选择。
- **待代码验证**：需要继续读取具体仓库实现。
- **待实验验证**：需要通过实验、Benchmark 或消融确认。

这样可以避免把讨论中的推测误写成已经实现的功能。