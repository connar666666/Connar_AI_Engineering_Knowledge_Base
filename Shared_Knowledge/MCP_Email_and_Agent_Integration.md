# Shared Knowledge：MCP、Email 与 Agent 集成

## 1. MCP Server 是什么

MCP Server 不是一个会主动理解所有业务需求的“服务商 Agent”，而是把外部能力以标准工具、资源或提示模板的形式提供给 MCP Client / Agent。

```text
User
  ↓
Agent / MCP Client
  ↓ MCP protocol
MCP Server
  ↓
Email、Database、Filesystem、API 等真实系统
```

Agent 决定何时调用工具；MCP Server 负责执行受控能力并返回结构化结果。

## 2. 自己编写 MCP Server

可以为自己的 Agent 编写 MCP Server，也可以让多个 Agent 或客户端复用同一个 MCP Server。

但“可复用”不等于“所有用户共享同一权限”。服务端仍需处理：

- 当前调用者身份。
- 账号作用域。
- 凭据引用。
- 权限检查。
- 并发与限流。
- 审计。

## 3. MCP Server 是否需要启动

取决于传输方式：

### stdio

Client 以子进程方式启动 Server，通过标准输入输出通信。

```text
Client process
  ↔ stdin/stdout
MCP Server process
```

这种方式通常不需要公开网络端口，但 Server 进程仍然需要运行。

### HTTP / Streamable HTTP

Server 作为网络服务运行，需要监听地址和端口。

```text
Agent Client
  ↔ HTTP
MCP Server
```

适合多个客户端、远程部署和统一运维，但需要认证、TLS、网关和限流。

## 4. Agent Loop 与 MCP Server 的关系

一个完整 Agent 应用可能同时包含：

```text
Agent Loop
- 理解目标
- 规划
- 选择工具
- 观察结果
- 继续执行

MCP Client
- 连接 Server
- 获取 Tool Schema
- 发起工具调用

MCP Server
- 执行真实操作
- 返回结构化结果
```

Agent Loop 和 MCP Server 可以在同一仓库，也可以分开部署。它们不是同一个概念。

## 5. Email MCP 示例

用户说：

```text
找到供应商昨天关于延期的邮件并帮我回复。
```

Agent 可能执行：

```text
search_emails
  ↓
read_email
  ↓
get_email_thread
  ↓
生成回复草稿
  ↓
create_draft
  ↓
用户确认
  ↓
send_draft
```

MCP Server 不应自己跳过确认并自动发送，除非调用策略明确允许。

## 6. Tool 与业务工作流

Tool 是原子能力：

```text
read_email
send_email
move_email
```

业务工作流是 Tool 的组合：

```text
“处理今日待回复邮件”
```

工作流可能由 Agent Prompt、Skill、状态机或后端 Orchestrator 实现。

## 7. MCP 与 HTTP 的关系

MCP 是应用层协议和工具交互规范；HTTP 可以是它的一种传输方式。

```text
MCP 定义：消息和能力如何表达
HTTP 定义：网络请求如何传输
```

就像业务 API 可以运行在 HTTP 上，MCP Server 也可以通过 stdio 或 HTTP 传输。

## 8. 认证与授权

认证回答“你是谁”，授权回答“你能做什么”。

Email MCP 中至少存在三层身份：

```text
OA 登录用户
邮箱账号
MCP 调用身份 / 应用身份
```

系统需要明确映射，不能让模型自由填写任意邮箱账号。

## 9. 状态与会话

MCP Tool 本身可以尽量无状态，但 Email 业务仍需要保存：

- 邮箱同步游标。
- 账号配置。
- 草稿。
- 审计日志。
- 幂等键。
- 线程索引。

“Tool 调用无状态”不代表整个业务系统不需要持久化。

## 10. 错误处理

MCP Server 应返回机器可理解的错误，例如：

```text
AUTH_FAILED
ACCOUNT_SCOPE_DENIED
MESSAGE_NOT_FOUND
RATE_LIMITED
ATTACHMENT_TOO_LARGE
SEND_REQUIRES_CONFIRMATION
TRANSIENT_NETWORK_ERROR
```

Agent 才能根据错误决定：

- 重新登录。
- 缩小查询范围。
- 请求用户确认。
- 稍后重试。
- 停止危险操作。

## 11. 设计结论

1. MCP Server 是能力提供层，不是完整 Agent。
2. 多个 Agent 可以复用同一个 Server，但必须隔离身份和权限。
3. Server 是否需要端口取决于 stdio 或 HTTP 传输。
4. Agent Loop、MCP Client 和 MCP Server 应分别理解。
5. Tool 要原子化，业务流程由 Agent 或 Orchestrator 组合。
6. 企业 Email MCP 的关键不是协议调用本身，而是认证、权限、线程、审计和安全确认。