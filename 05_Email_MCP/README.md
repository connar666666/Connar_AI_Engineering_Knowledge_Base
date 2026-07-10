# 05_Email_MCP

这个目录整理企业内部 OA Agent 的 Email MCP 接入项目。

## 项目目标

让用户在 OA Agent 中通过自然语言完成：

- 查询收件箱和未读邮件。
- 搜索历史邮件。
- 读取完整邮件和附件。
- 发送、回复、转发邮件。
- 按会话追踪历史往来。
- 汇总待办、风险、审批和项目进展。

## 核心边界

Email MCP 负责把邮箱能力标准化为 Agent 可调用的工具，但不应该单独承担整个企业身份体系。

```text
企业微信 / OA SSO
  ↓ 用户身份
OA Agent 权限层
  ↓ 邮箱账号映射与调用授权
Email MCP
  ↓ IMAP / SMTP / 内部邮箱 API
集团邮箱服务器
```

## 当前文档

```text
05_Email_MCP/
├── README.md
├── Business_Requirements_and_Enterprise_Scenarios.md
├── Protocol_Threading_Authentication_and_Permissions.md
├── Architecture_Tool_List_Skills_and_Testing.md
└── Mail_Server_Questionnaire.md
```

## 当前阶段

- **已确认**：集团邮箱可通过 IMAP/SMTP 和第三方客户端授权方式访问。
- **设计结论**：身份认证、账号映射、权限和审计主要由 OA Agent 平台负责。
- **待确认**：邮箱服务器是否支持 EWS、内部 API、THREAD、QRESYNC、IDLE、并发限制和统一授权能力。
- **待代码验证**：`Email_MCP` 仓库 `emails/` 目录的最终工具覆盖、测试覆盖和 Skills 组织。