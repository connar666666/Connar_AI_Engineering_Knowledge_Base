# 项目恢复索引

本文件只负责把当前问题路由到正确的项目恢复文件，不保存各项目详细状态。

| 项目 | 恢复文件 | 对应知识库 |
|---|---|---|
| AAAI 可读性 | `Projects/01_AAAI_Legibility_Committee/RECOVERY_STATE.md` | `01_AAAI_Legibility_Committee/` |
| ComfyUI WorkBench / FrameWeave | `Projects/02_ComfyUI_Bench/RECOVERY_STATE.md` | `02_ComfyUI_Bench/` |
| OpenAI Agent Attack | `Projects/03_OpenAI_Agent_Attack/RECOVERY_STATE.md` | `03_OpenAI_Agent_Attack/` |
| AgentOS / OS Agent | `Projects/04_AgentOS_OSAgent/RECOVERY_STATE.md` | `04_AgentOS_OSAgent/` |
| Email MCP | `Projects/05_Email_MCP/RECOVERY_STATE.md` | `05_Email_MCP/` |
| SZU_TK / Transfer Agent | `Projects/06_SZU_TK/RECOVERY_STATE.md` | `06_SZU_TK/` |
| 手撕代码 / Python | `Projects/07_Hand_Coding/RECOVERY_STATE.md` | `07_Hand_Coding/` |
| 实习与面试 | `Projects/08_Internship/RECOVERY_STATE.md` | `08_Internship/` |
| 工程环境与工具 | `Projects/Engineering/RECOVERY_STATE.md` | `Engineering/` |

## 使用规则

1. 根据用户当前问题选择唯一主要项目。
2. 读取该项目 `RECOVERY_STATE.md`。
3. 按其中的“知识库加载清单”继续读取现有知识库。
4. 跨项目问题只在确实存在依赖时，额外加载另一个项目恢复文件。
5. 不要一次加载全部项目。

## 常见路由

- 邮箱、IMAP、SMTP、thread、Email Tool、Skills → `05_Email_MCP`
- Transfer Agent、时间线、Snapshot、KV Cache、Remotion → `06_SZU_TK`
- LBF、Rule Discovery、Reward Shaping、论文实验 → `01_AAAI_Legibility_Committee`
- Prompt 攻击、Mutation、Replay、Harness、Rubric → `03_OpenAI_Agent_Attack`
- Project / Canvas、邀请、权限、ComfyUI → `02_ComfyUI_Bench`
- Quickselect、LRU、链表、asyncio、线程进程 → `07_Hand_Coding`
- 面试复盘、项目表达、CI、Test、Lint → `08_Internship`
- AgentOS、OS Agent、系统级 Agent → `04_AgentOS_OSAgent`
- WSL、Docker、Git、Codex、Claude Code、代理 → `Engineering`