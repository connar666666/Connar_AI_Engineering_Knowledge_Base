# Python：多线程、多进程、协程与异步

## 1. 先从真实服务场景理解

假设一个后端服务需要完成：

1. 接收用户上传的 100 个文件。
2. 从对象存储下载文件。
3. 解析文本。
4. 调用多个外部模型 API。
5. 对部分图片做 CPU 密集型特征计算。
6. 把结果写入数据库。

不同任务适合不同并发方式：

```text
网络下载 / API 调用 / 数据库等待 → asyncio
阻塞但线程安全的旧 SDK         → 线程池
CPU 密集型计算                  → 进程池 / 多进程
服务级隔离和扩容                → 多个进程或多个容器
```

这不是四选一，而是可以组合。

## 2. 进程与线程

### 进程

进程拥有相对独立的内存空间和 Python 解释器。

优点：

- 可以利用多个 CPU 核心。
- 故障隔离更好。
- 适合 CPU 密集型任务。

代价：

- 创建和切换成本更高。
- 进程间通信更复杂。
- 数据需要序列化或共享内存。

### 线程

同一进程内的线程共享内存。

优点：

- 创建成本较低。
- 共享数据方便。
- 适合阻塞型 I/O。

风险：

- 需要锁保护共享状态。
- 容易出现竞态条件和死锁。
- CPython 的 GIL 限制纯 Python CPU 密集型线程并行。

## 3. GIL

GIL 是 CPython 解释器中的全局解释器锁。它使同一进程内通常只有一个线程在执行 Python 字节码。

因此：

- 多线程仍然可以提升 I/O 密集任务，因为线程等待网络或磁盘时会让出执行机会。
- 多线程通常不能让纯 Python CPU 密集任务线性利用多个核心。
- 使用 NumPy 等底层 C 扩展时，某些操作会释放 GIL，情况可能不同。
- CPU 密集任务通常使用多进程或原生扩展。

## 4. `start()` 与 `join()`

### `start()`

启动一个线程或进程，让它开始执行目标函数。

### `join()`

让当前调用者等待目标线程或进程结束。

`join()` 不是“结束任务”，而是“等待任务结束”。

```text
main thread
  ├── start worker
  ├── main 可以继续做别的事
  └── join worker → 在这里等待 worker 完成
```

如果不需要等待结果，可以不立刻 `join()`；但主程序退出前通常需要正确回收资源。

## 5. 锁与竞态条件

当多个线程同时修改共享数据时：

```python
counter += 1
```

并不是不可分割的单一步骤，可能产生丢失更新。

锁的基本思想：

```text
线程 A 获得锁
  ↓
修改共享状态
  ↓
释放锁
  ↓
线程 B 才能进入
```

应尽量缩小临界区，不要在持锁时进行慢速网络请求。

## 6. Queue

Queue 用于生产者和消费者之间安全传递任务。

```text
Producer → Queue → Worker
```

它比多个线程直接修改同一个列表更安全，因为 Queue 通常已经封装了同步机制。

常见场景：

- 下载线程产生待解析文件。
- Worker 从队列消费任务。
- 主线程等待所有任务完成。

## 7. 线程池和进程池

池解决的是“复用一组 Worker”，避免为每个任务反复创建线程或进程。

### 线程池

适合：

- 阻塞网络 SDK。
- 文件 I/O。
- 无法改成原生异步的第三方库。

### 进程池

适合：

- 图像处理。
- 复杂文本计算。
- 纯 Python CPU 密集算法。

池并不会改变任务本质。CPU 密集任务放进线程池，仍然可能受 GIL 限制。

## 8. 为什么有线程池还需要 asyncio

线程在 I/O 等待时虽然可以切换，但每个线程仍然有：

- 栈内存。
- 调度成本。
- 锁和共享状态风险。

当服务需要同时维护成百上千个网络连接时，创建同样数量的线程成本较高。

`asyncio` 使用事件循环：

```text
协程发起 I/O
  ↓ await
把执行权交还事件循环
  ↓
事件循环运行其他协程
  ↓
I/O 完成后恢复原协程
```

这是一种协作式调度，不需要为每个连接创建一个系统线程。

## 9. Coroutine、Task、Future

### Coroutine

调用 `async def` 函数得到的协程对象，描述一段可暂停、可恢复的异步计算。

```python
async def fetch():
    ...

coro = fetch()
```

此时不一定已经开始执行。

### Task

Task 把协程注册到事件循环中调度。

```python
task = asyncio.create_task(fetch())
```

Task 是“正在由事件循环管理的协程执行实例”。

### Future

Future 表示一个未来会得到结果或异常的占位对象。

```text
pending → finished(result)
pending → finished(exception)
```

Task 是 Future 的一种高级形式，它会驱动协程执行并最终保存结果。

## 10. Python 与 TypeScript 异步的对应

| TypeScript | Python |
|---|---|
| `async function` | `async def` |
| Promise | Coroutine / Task / Future 的组合语义 |
| `await promise` | `await coroutine_or_task` |
| `Promise.all` | `asyncio.gather` |
| Event Loop | Event Loop |

TypeScript 中调用 `async function` 会立即返回 Promise；Python 中调用 `async def` 返回 coroutine，通常需要 `await` 或创建 Task 才执行。

## 11. Promise 是什么

Promise 是对未来结果的封装：

```text
pending
  ↓
fulfilled(value)
或
rejected(error)
```

它让异步代码可以使用链式调用或 `await`，而不是层层回调。

## 12. 真实组合示例

一个 FastAPI 请求需要处理多个文件：

```text
FastAPI / asyncio event loop
  ├── 异步下载对象存储文件
  ├── 异步调用 LLM API
  ├── 在线程池调用阻塞 PDF SDK
  └── 在进程池执行 CPU 密集图像分析
```

伪代码：

```python
async def handle_job(files):
    downloaded = await asyncio.gather(*(download(f) for f in files))

    loop = asyncio.get_running_loop()
    texts = await asyncio.gather(*(
        loop.run_in_executor(thread_pool, parse_blocking_pdf, f)
        for f in downloaded
    ))

    features = await asyncio.gather(*(
        loop.run_in_executor(process_pool, cpu_heavy_feature, t)
        for t in texts
    ))

    return await call_model(features)
```

这里：

- 主流程使用 asyncio 组织并发。
- 阻塞库放线程池，避免卡住事件循环。
- CPU 密集任务放进程池。

## 13. 常见错误

- 在 `async def` 中直接调用长时间阻塞函数。
- 创建协程但没有 `await`。
- 创建 Task 后不保存引用或不处理异常。
- 无限制 `gather` 导致连接和内存爆炸。
- 多线程修改共享字典却没有同步。
- 进程池参数无法 pickle。
- 在每个请求里新建线程池或进程池。
- 把并发误认为一定更快，忽略外部限流。

## 14. 面试回答框架

```text
I/O 密集：优先 asyncio；阻塞库可用线程池
CPU 密集：多进程或进程池
线程：共享内存方便，但要处理锁和 GIL
进程：可利用多核，但通信成本高
asyncio：单线程事件循环下的协作式并发
```

最终选择取决于任务是等待型、计算型，还是需要服务级隔离。