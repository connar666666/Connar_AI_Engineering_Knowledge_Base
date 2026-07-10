# 实习面试：Test、CI、Lint 与全栈知识

## 1. PR 中的 Test、CI、Lint 分别是什么

### Test

Test 验证程序行为是否符合预期。

常见层级：

- Unit Test：单个函数、类或模块。
- Integration Test：数据库、Redis、外部服务或多个模块协作。
- End-to-End Test：从用户操作到最终结果。

### Lint

Lint 静态检查代码质量和风格，例如：

- 未使用变量。
- 潜在类型错误。
- 不规范 import。
- 代码风格不一致。
- React Hook 依赖错误。

Lint 通常不运行真实业务流程，因此“lint 通过”不代表功能正确。

### CI

CI 是自动执行检查的流水线。它可以包含：

```text
安装依赖
  ↓
lint
  ↓
type check
  ↓
unit test
  ↓
integration test
  ↓
build
```

因此 CI 不是和 test、lint 并列的某一种代码检查；CI 是承载这些检查的自动化机制。

## 2. 为什么 PR 需要自动检查

PR 检查的目标是防止：

- 新代码破坏旧功能。
- 依赖无法安装。
- 前端无法构建。
- 类型定义不一致。
- 数据库迁移遗漏。
- 格式和静态错误进入主分支。

它不能完全替代人工 Review，因为架构合理性、业务逻辑和安全风险仍需要人判断。

## 3. FastAPI 在一次请求中的作用

以聊天或 Agent 请求为例：

```text
Browser
  ↓ HTTP POST
Nginx / Load Balancer
  ↓
FastAPI Route
  ↓
Pydantic Schema Validation
  ↓
Service Layer
  ├── Redis
  ├── SQL Database
  ├── Agent / LLM
  └── Queue / Worker
  ↓
JSON / Streaming Response
```

FastAPI 不是数据库，也不是服务器硬件。它是 Python 后端框架，用于：

- 注册路由。
- 解析 HTTP 请求。
- 校验输入。
- 调用业务逻辑。
- 返回响应。

生产中可以在多个 Docker 容器或多个进程中运行多个 FastAPI 实例，再由负载均衡分发请求。

## 4. Redis 的作用

Redis 是内存型 Key-Value 数据系统，不是传统关系型表格数据库。

适合：

- Session。
- 缓存。
- 限流计数。
- 分布式锁。
- 短期任务状态。
- 发布订阅。
- 队列或队列后端。

不适合把所有长期核心业务数据只保存在 Redis 中。

## 5. SQL 数据库的作用

SQL 数据库保存长期、结构化、需要事务和关系约束的数据，例如：

- 用户。
- 项目。
- 成员权限。
- 会话。
- 消息元数据。
- Agent 运行记录。
- 版本和审计记录。

除了 CRUD，还需要理解：

- 主键、外键。
- 唯一约束。
- 事务。
- 隔离级别。
- 索引。
- 联合索引。
- 查询计划。
- 锁。
- 分页。
- 软删除与归档。

## 6. 联合索引

假设常见查询是：

```sql
SELECT *
FROM emails
WHERE account_id = ?
  AND mailbox = ?
  AND sent_at < ?
ORDER BY sent_at DESC
LIMIT 50;
```

可以考虑联合索引：

```text
(account_id, mailbox, sent_at)
```

联合索引不是把几个单列索引简单相加。列顺序取决于：

- 查询中的等值条件。
- 范围条件。
- 排序。
- 数据选择性。

要通过真实查询和执行计划验证，不能只背“最左前缀”。

## 7. Docker 的作用

Docker 镜像包含应用运行所需的文件系统、依赖和启动配置；容器是镜像的运行实例。

```text
Dockerfile → Image → Container
```

一个服务可能有多个容器实例：

```text
fastapi-api-1
fastapi-api-2
worker-1
worker-2
redis
postgres
```

镜像不是按项目终端保存的，而是由本机 Docker Engine 全局管理。某个目录中 `docker pull` 成功后，其他目录通常也能使用同一个镜像。

## 8. 一次 Agent 请求中 Redis 与 SQL 如何配合

```text
用户发送消息
  ↓
FastAPI 校验用户身份
  ↓
SQL 读取会话和长期消息记录
  ↓
Redis 读取短期会话状态、缓存或限流信息
  ↓
调用 Agent / LLM
  ↓
流式结果暂存或发布到 Redis
  ↓
最终消息和运行记录写入 SQL
  ↓
返回前端
```

Redis 解决速度和短期状态，SQL 解决长期一致性和关系。

## 9. 面试中如何解释“服务”

`FastAPI service` 通常指一个承担特定后端职责的应用服务，而不是某个固定 Docker 容器。

```text
代码层：FastAPI application
运行层：process
部署层：container / pod
网络层：service endpoint / load balancer
```

一个逻辑服务可以部署多个容器实例。

## 10. 常见面试追问

### 为什么不直接把任务放数据库

数据库可以保存任务记录，但高频队列消费、状态更新和实时通知通常使用 Redis 或专门消息队列更合适。最终状态仍写入 SQL。

### Redis 挂了怎么办

- 缓存可重建。
- 关键数据不只保存在 Redis。
- 使用持久化、主从或集群。
- 任务需要幂等和恢复机制。

### FastAPI 为什么适合 Agent 后端

- Python AI 生态丰富。
- Pydantic schema 清晰。
- 原生支持异步。
- 易于实现流式响应。
- 可以和队列、模型 SDK、数据库集成。

但高吞吐能力仍取决于部署、阻塞调用、数据库和外部模型延迟。

## 11. 回答原则

面试中不要把技术逐个孤立解释。应通过一次真实请求说明：

```text
请求从哪里来
经过哪些服务
数据存在哪里
慢任务如何处理
结果如何返回
故障如何恢复
```

能够讲清数据流，才说明真正理解了全栈系统。