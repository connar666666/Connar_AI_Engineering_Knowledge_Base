# ComfyUI_Bench：后端理解

## 1. FastAPI 在项目中的作用

FastAPI 是后端的请求处理中心。前端发来的 HTTP 请求会进入 FastAPI，由路由函数处理。

典型链路：

```text
前端按钮点击
  ↓
HTTP 请求
  ↓
FastAPI 路由
  ↓
业务逻辑 Service
  ↓
数据库 / Redis / OSS / Worker
  ↓
返回 JSON
```

## 2. 路由装饰器的理解

FastAPI 中常见代码：

```python
@app.get("/projects/{project_id}")
def get_project(project_id: int):
    ...
```

这里的 `@app.get(...)` 是路由装饰器。它的含义是：

```text
当后端收到 GET /projects/{project_id} 请求时，调用下面这个 Python 函数处理。
```

所以路由装饰器就是把：

```text
HTTP 方法 + URL 路径 + Python 函数
```

绑定起来。

## 3. Redis 在后端中的作用

Redis 不是 SQL 数据库。它是内存型 Key-Value 存储，适合处理高频、短生命周期、临时状态类数据。

在本项目中，Redis 可以用于：

- 任务队列。
- 生成任务状态。
- 缓存热点数据。
- 限流。
- Session。
- Worker 与 API 服务之间的状态同步。

例如：

```text
submit generation task
  ↓
write task status to Redis
  ↓
worker consumes task
  ↓
update progress in Redis
  ↓
frontend polls or subscribes status
```

## 4. SQL 在后端中的作用

SQL 数据库用于保存长期结构化数据，例如：

- users
- projects
- project_members
- canvas
- canvas_members
- canvas_versions
- canvas_outputs
- assets
- oss_objects
- invitations

SQL 适合描述实体和关系；Redis 更适合描述短期状态。

## 5. 后端分层建议

推荐分层：

```text
api/        路由层
schemas/    请求与响应模型
services/   业务逻辑
models/     ORM 数据模型
repositories/ 数据访问
workers/    后台任务
core/       配置、鉴权、数据库连接
```

## 6. 典型 API

### Project

```text
GET    /projects
POST   /projects
GET    /projects/{id}
PATCH  /projects/{id}
DELETE /projects/{id}
```

### Canvas

```text
GET    /projects/{project_id}/canvas
POST   /projects/{project_id}/canvas
GET    /canvas/{canvas_id}
PATCH  /canvas/{canvas_id}
DELETE /canvas/{canvas_id}
```

### Invite

```text
POST /projects/{project_id}/invites
POST /canvas/{canvas_id}/invites
GET  /me/invites
POST /invites/{invite_id}/accept
POST /invites/{invite_id}/decline
```

## 7. 后端理解重点

这个项目中的后端不是简单 CRUD，而是要处理：

1. 用户权限。
2. Project 与 Canvas 层级关系。
3. 邀请状态。
4. 任务异步执行。
5. 大文件对象存储。
6. 生成结果回写。
7. 前端实时状态展示。

