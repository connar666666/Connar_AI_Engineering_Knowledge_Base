# Email MCP 对话恢复状态

## 项目目标
在集团 OA Agent 中接入企业邮箱，支持查询、搜索、收发、回复、转发、历史追踪和会话聚合。

## 对话历史关键点
- **用户提出**：企业内部大量非固定流程依赖邮件，需求不能只停留在基础收发。
- **用户提出**：希望复用 OA / 企业微信 SSO，减少用户逐个配置邮箱授权。
- **用户关心**：IMAP 是否能直接按 thread 获取完整会话。
- **助手补充**：需区分 OA 身份、邮箱账号、邮箱凭据；不能假设服务器一定有原生 thread id。
- **共同结论**：OA Agent 管身份、策略、确认和审计；Email MCP 管协议和工具；邮箱服务器管账号、数据和服务端限制。

## 当前共同结论
- SMTP 负责发送，IMAP 负责读取和同步。
- 线程优先依据 `Message-ID`、`In-Reply-To`、`References` 构建。
- 协议线程与业务语义线程必须分开。
- Tool 是原子能力，Skill 是多工具工作流，安全边界不能只放在 Skill。
- 发送类工具需要确认、审计和幂等。

## 已确认事实
- 第三方客户端专用密码可用于 IMAP / SMTP。
- 已形成邮箱服务器能力确认清单。

## 待验证
- `Email_MCP/emails/` 中真实 Tool 注册位置、Service 分层、thread 实现和测试覆盖。
- `prompts/skills` 是否具备完整工作流。
- fork 的 `mcp-email-server` 与自研实现的工具差异。
- 邮箱服务器对 IDLE、QRESYNC、THREAD、并发、统一授权的支持。

## 知识库加载清单
按问题选择读取：

```text
05_Email_MCP/README.md
05_Email_MCP/Business_Requirements_and_Enterprise_Scenarios.md
05_Email_MCP/Protocol_Threading_Authentication_and_Permissions.md
05_Email_MCP/Architecture_Tool_List_Skills_and_Testing.md
05_Email_MCP/Mail_Server_Questionnaire.md
Shared_Knowledge/MCP_Email_and_Agent_Integration.md
```

## 恢复后继续位置
优先读取真实 `Email_MCP` 仓库指定分支下 `emails/`，确认当前 Tool List、线程能力、测试与 Skills，再更新本状态。