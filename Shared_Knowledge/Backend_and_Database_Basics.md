# Shared Knowledge：后端与数据库基础

## 1. FastAPI、HTTP、Redis、SQL 的关系

一次真实 Web 请求可以理解为：

```text
浏览器前端
  ↓ HTTP
Nginx / 网关
  ↓
FastAPI
  ↓
业务逻辑
  ↓
Redis / SQL / OSS / Worker
  ↓
返回响应
```

## 2. FastAPI

FastAPI 是 Python 后端框架，用来接收 HTTP 请求并调用对应的 Python 函数处理。

它不是数据库，也不是服务器硬件，而是运行在服务器上的应用服务。

## 3. HTTP

HTTP 是前端和后端通信的协议。

常见方法：

```text
GET     获取数据
POST    创建数据
PATCH   修改数据
DELETE  删除数据
```

## 4. Redis

Redis 是内存型 Key-Value 数据库。它不是 SQL 表格数据库。

适合：

- 缓存。
- Session。
- 队列。
- 分布式锁。
- 限流。
- 临时状态。

## 5. SQL

SQL 数据库适合保存长期结构化数据。

除了增删改查，还需要理解：

- 表关系。
- 主键。
- 外键。
- 索引。
- 联合索引。
- 唯一约束。
- 事务。
- 隔离级别。
- 查询优化。

## 6. 装饰器与 FastAPI 路由

普通装饰器是给函数增加额外行为。

FastAPI 的路由装饰器则用于把 HTTP 请求和 Python 函数绑定。

```python
@app.post("/projects")
def create_project():
    ...
```

意思是：当后端收到 `POST /projects` 请求时，执行这个函数。

