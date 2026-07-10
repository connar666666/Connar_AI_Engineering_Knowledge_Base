# FastAPI、HTTP、Redis、SQL 全链路说明

> 状态：历史重建版。根据过去多轮对话和现有知识文档重建，不是旧工作区逐字原文。

## 1. 场景

用户在网页中向一个 Agent 发送消息：

```text
“读取我的项目状态，调用模型生成修改建议，并把结果保存下来。”
```

一次完整请求通常经过：

```text
Browser
→ HTTP
→ Nginx / Load Balancer
→ FastAPI Route
→ Authentication and Schema Validation
→ Service Layer
→ Redis / SQL / Object Storage / Queue / LLM
→ Streaming or JSON Response
```

## 2. FastAPI 是什么

FastAPI 是 Python 后端框架。它负责接收 HTTP 请求、解析参数、校验输入、调用业务逻辑并返回响应。

```python
@app.post('/projects/{project_id}/agent/messages')
async def send_agent_message(project_id: str, request: AgentRequest):
    return await agent_service.handle(project_id, request)
```

路由函数不应塞入全部业务逻辑。生产代码通常继续调用 Service、Repository 和外部 Client。

## 3. 一个逻辑服务与多个容器

```text
逻辑层：FastAPI application
运行层：Python process
部署层：Docker container / Kubernetes pod
网络层：Service / Load Balancer
```

同一套 FastAPI 代码可以运行多个容器实例。负载均衡器把不同请求分发给不同实例。

## 4. Redis 的职责

Redis 是内存型 Key-Value 数据系统，适合：

- Session 和短期登录状态。
- 高频缓存。
- 限流计数。
- 分布式锁。
- 临时 Agent Run 状态。
- 发布订阅和流式进度。
- 任务队列的 Broker 或 Backend。

Redis 不适合成为所有长期核心业务数据的唯一存储。

## 5. SQL 数据库的职责

PostgreSQL、MySQL 等关系数据库保存长期、结构化且需要事务的数据：

- 用户和组织。
- Project、Canvas 和成员权限。
- 会话、消息和 Agent Run。
- 版本、事件、审计记录。
- 任务最终状态。

除了 CRUD，还要理解：

- 主键、外键和唯一约束。
- 事务与隔离级别。
- 单列索引与联合索引。
- 查询计划。
- 锁和并发更新。
- 分页、软删除和归档。

## 6. Redis 与 SQL 如何配合

```text
用户请求
→ FastAPI 校验身份
→ SQL 读取长期项目、权限和历史记录
→ Redis 检查 Session、限流、缓存或运行状态
→ 调用 Agent / LLM
→ Redis 发布流式 token 或进度
→ SQL 保存最终消息、版本和审计记录
→ 返回前端
```

核心原则：

```text
Redis 解决速度和短期状态
SQL 解决长期一致性和关系
```

## 7. 慢任务为什么需要队列

视频渲染、大文件处理、批量推理等任务不应一直占用 HTTP 请求：

```text
POST /jobs
→ SQL 创建 job
→ Queue 投递任务
→ Worker 消费
→ Redis 更新进度
→ SQL 写最终状态
→ 前端轮询或订阅
```

API 服务负责接收请求，Worker 负责执行长任务。

## 8. 联合索引示例

```sql
SELECT *
FROM messages
WHERE conversation_id = ?
  AND created_at < ?
ORDER BY created_at DESC
LIMIT 50;
```

可考虑：

```sql
CREATE INDEX idx_messages_conversation_time
ON messages(conversation_id, created_at DESC);
```

索引列顺序必须根据真实查询、选择性和执行计划验证，不能只背规则。

## 9. 一致性与幂等

高风险写操作需要：

- 幂等键，避免重试造成重复提交。
- 数据库事务，保证多表修改要么全部成功，要么全部回滚。
- 版本号或乐观锁，避免覆盖其他用户的新修改。
- 明确错误分类，让前端或 Agent 知道是否可重试。

## 10. 面试表达

不要只说“项目用了 FastAPI、Redis、SQL、Docker”。完整回答应说明：

```text
请求从哪里来
→ FastAPI 如何接住
→ Redis 保存什么
→ SQL 保存什么
→ 慢任务怎样进入 Worker
→ 结果怎样返回
→ 失败如何恢复
```

只有能讲清数据流和边界，才说明真正理解了全栈后端。