# Email MCP：历史生成文档归档

## 权威成品

| 历史交付主题 | 权威文件 | 状态 |
|---|---|---|
| 企业邮件业务场景说明与需求边界 | `../Business_Requirements_and_Enterprise_Scenarios.md` | 权威成品 |
| SMTP、IMAP、线程、认证和权限设计 | `../Protocol_Threading_Authentication_and_Permissions.md` | 权威成品 |
| 架构、Tool List、Skills 与测试设计 | `../Architecture_Tool_List_Skills_and_Testing.md` | 权威成品 |
| 邮箱服务器能力确认问题清单 | `../Mail_Server_Questionnaire.md` | 权威成品 |

## 历史重建成品

| 文档 | 说明 |
|---|---|
| `Mail_Server_Consultation_Email_FINAL_REBUILT.md` | 根据最终确认清单重建的可直接发送邮件版本 |

## 历史请求来源

用户曾明确要求：

- 补齐集团内部 Email MCP 的业务场景说明书。
- 覆盖非固定流程，而不只是基础收发。
- 合并面向邮箱服务器同事的两版问题邮件。
- 只要求对方确认服务器能力，技术方案由项目组决定。
- 检查 Tool List、线程能力、Skills 和测试覆盖。

## 尚未归档为真实代码报告

以下内容仍需读取具体代码仓库后才能生成：

- `Email_MCP/emails/` 当前完整架构审计。
- Tool 注册位置和真实 Tool List。
- 与 fork `mcp-email-server` 的逐工具差异表。
- Skills 重构后的最终 Prompt 文件。

现有架构文档是设计成品，不代表所有功能已经落地。