# Email MCP：架构、Tool List、Skills 与测试设计

## 1. 推荐目录职责

结合当前讨论，`emails/` 应该围绕“协议适配—领域模型—MCP 工具—Agent Skills—测试”分层，而不是把所有逻辑写在单个 Server 文件中。

```text
emails/
├── app/ or src/
│   ├── config/           配置和环境变量
│   ├── auth/             账号上下文、凭据引用和权限检查
│   ├── clients/          IMAP、SMTP、可能的 EWS/API 客户端
│   ├── domain/           Message、Thread、Attachment、Mailbox 模型
│   ├── services/         搜索、线程聚合、发送、同步等业务逻辑
│   ├── tools/            暴露给 MCP 的工具定义
│   ├── server/           MCP Server 启动和工具注册
│   └── storage/          游标、缓存、索引和审计持久化
├── prompts/
│   └── skills/           Agent 如何组合工具完成业务任务
└── tests/
    ├── unit/
    ├── integration/
    ├── contract/
    └── fixtures/
```

具体目录名需要以仓库实现为准，但职责不应混淆。

## 2. Tool 和 Skill 的区别

### Tool

Tool 是一个可执行、参数明确、结果结构化的原子能力，例如：

```text
search_emails
read_email
send_email
reply_email
list_mailboxes
read_attachment
```

Tool 必须能够被独立测试。

### Skill

Skill 是 Agent 使用一组 Tool 完成更高层任务的操作说明，例如：

```text
“汇总今天需要我回复的邮件”
```

可能需要组合：

```text
search_emails
  ↓
read_email_batch
  ↓
group_threads
  ↓
extract_action_items
  ↓
生成摘要
```

因此：

```text
Tool = 能做什么
Skill = 为完成某类目标，应该如何规划和调用 Tool
```

Skills 不能替代 Tool 的参数校验、权限检查和错误处理。

## 3. 推荐 Tool List

### 邮箱和文件夹

- `list_accounts`
- `list_mailboxes`
- `get_mailbox_status`

### 搜索和读取

- `search_emails`
- `list_emails`
- `read_email`
- `read_emails_batch`
- `get_email_headers`
- `get_email_thread`

### 附件

- `list_attachments`
- `download_attachment`
- `save_attachment`

### 写操作

- `create_draft`
- `update_draft`
- `send_draft`
- `send_email`
- `reply_email`
- `reply_all`
- `forward_email`

### 状态和组织

- `mark_read`
- `mark_unread`
- `flag_email`
- `move_email`
- `delete_email`

### 同步和诊断

- `sync_mailbox`
- `get_sync_status`
- `get_server_capabilities`
- `health_check`

## 4. Tool 参数设计原则

不要让模型直接拼接底层 IMAP 命令。Tool 应提供受控参数：

```json
{
  "account_id": "current_user",
  "mailbox": "INBOX",
  "from": "supplier@example.com",
  "subject_contains": "delivery",
  "after": "2026-07-01",
  "before": "2026-07-10",
  "has_attachment": true,
  "limit": 50
}
```

发送类 Tool 建议包含：

```text
idempotency_key
confirmation_token
sensitivity_level
```

避免 Agent 因重试造成重复发送。

## 5. 返回结构设计

搜索列表不应直接返回所有正文和附件，以免撑爆上下文。

### 摘要结果

```text
email_id
thread_id
subject
from
recipients
sent_at
snippet
flags
attachment_count
```

### 读取详情

用户或 Agent 确认需要后，再调用 `read_email` 获取：

- 完整正文。
- HTML / text 版本。
- 邮件头。
- 附件元数据。
- 线程关系。

## 6. 会话级 Tool

开源邮件 MCP 中常见的会话能力值得借鉴，但不能只通过主题字符串实现。

推荐 `get_email_thread` 接收：

```text
email_id 或 message_id
include_bodies
include_attachments
max_messages
sort_order
```

返回：

```text
thread_id
thread_source: server | header_graph | inferred
confidence
messages[]
participants[]
first_message_at
last_message_at
```

## 7. Skills 设计建议

每个 Skill 至少包含：

1. 使用场景。
2. 前置条件。
3. 允许调用的 Tool。
4. 推荐调用顺序。
5. 失败分支。
6. 用户确认点。
7. 安全限制。
8. 输出格式。

示例 Skill：回复邮件

```text
1. 读取目标邮件及线程。
2. 识别用户要回应的问题。
3. 必要时检索相关历史邮件。
4. 生成草稿，不直接发送。
5. 展示 To/Cc/Subject/Body/Attachments。
6. 用户确认后调用 send_draft。
7. 返回发送结果和审计编号。
```

## 8. 基础测试矩阵

### Unit Test

- 邮件头解析。
- MIME 正文选择。
- 附件文件名解码。
- Message-ID 标准化。
- 线程建图。
- 主题规范化。
- 参数校验。
- 错误映射。

### Integration Test

- IMAP 登录和退出。
- 列出文件夹。
- UID 搜索与拉取。
- SMTP 发送。
- 回复头生成。
- 附件上传和下载。
- 断线重连。

### Contract Test

- Tool schema 是否稳定。
- Tool 返回字段是否符合 MCP 声明。
- 错误是否使用统一结构。
- 敏感字段是否不会出现在模型响应中。

### End-to-End Test

- 搜索并总结邮件。
- 从单封邮件恢复完整线程。
- 创建草稿并确认发送。
- 回复带附件的邮件。
- 用户 A 不能访问用户 B 的邮件。
- 重试不会重复发送。

## 9. 当前实现检查重点

后续读取 `Email_MCP/emails/` 时，应重点确认：

- Tool 注册入口究竟位于哪里。
- 工具是直接调用 IMAP/SMTP，还是经过 Service 层。
- 当前是否有真正的 thread 模型。
- `prompts/skills` 是否只是几句提示词，还是包含完整工作流。
- 是否有发送确认、账号隔离和幂等。
- 单元测试与集成测试分别覆盖哪些能力。
- fork 的 `mcp-email-server` 中有哪些工具尚未迁移。

## 10. 架构结论

1. Tool 必须保持原子、结构化和可独立测试。
2. Skill 负责多工具编排，但不能承担安全边界。
3. 搜索结果和完整正文应分层返回。
4. 线程能力需要领域模型，不能只是临时查询函数。
5. 发送类操作必须具备确认、审计和幂等机制。