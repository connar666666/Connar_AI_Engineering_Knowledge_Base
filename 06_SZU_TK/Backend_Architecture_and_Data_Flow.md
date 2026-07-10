# SZU_TK：后端架构与 Agent 数据流

## 1. 系统角色

结合对话，系统至少包含以下角色：

```text
Web Frontend
Backend API
Transfer Agent
LLM Provider
Project / Timeline Storage
Asset Storage
Remotion Preview / Renderer
```

它们的职责应明确分离。

## 2. 前端职责

前端负责：

- 接收用户输入。
- 展示当前项目和时间线。
- 收集 Agent 所需的当前状态。
- 调用后端 API。
- 展示 Agent 的计划、修改结果和错误。
- 本地或服务端触发 Remotion 预览。

前端不应该独自承担：

- 权限校验。
- Agent 操作的最终合法性判断。
- 关键数据持久化的一致性。
- 任意代码或任意组件执行。

## 3. 后端职责

后端应作为可信执行边界：

```text
API Route
  ↓
Auth / Project Permission
  ↓
Request Schema Validation
  ↓
Agent Orchestration Service
  ↓
Operation Validation
  ↓
State Transaction / Persistence
  ↓
Return New Version and Diff
```

后端需要完成：

- 用户与项目权限校验。
- 请求参数校验。
- 加载当前版本。
- 构造受控 Agent Context。
- 调用 LLM 或 Agent Loop。
- 校验 Agent 输出的操作。
- 应用操作并生成新版本。
- 保存事件日志和 Snapshot。
- 返回差异，而不是每次返回不受控制的全量历史。

## 4. Transfer Agent 的输入

推荐把输入拆成稳定部分和变化部分。

### 稳定前缀

- 系统目标。
- 工具定义。
- 输出 schema。
- 安全规则。
- 时间线数据模型说明。

### 当前状态

- 当前项目版本号。
- 当前时间线的规范化状态。
- 当前选中元素。
- 当前可用资产索引。
- 必要的局部上下文。

### 最近交互

- 最近几轮用户意图。
- Agent 已执行操作摘要。
- 未解决约束。

不要把所有历史 Snapshot 原封不动拼入输入。

## 5. Agent 输出

相比直接生成一份新的超大 JSON，更推荐结构化操作：

```json
{
  "base_version": 42,
  "intent": "replace_intro_and_adjust_music",
  "operations": [
    {
      "op": "replace_clip",
      "target_id": "clip_001",
      "asset_id": "asset_123"
    },
    {
      "op": "set_duration",
      "target_id": "clip_001",
      "duration_ms": 4200
    }
  ],
  "explanation": "..."
}
```

优点：

- 可以逐项校验。
- 可以生成 diff。
- 可以重放和撤销。
- 更容易记录审计日志。
- 减少模型复制整个状态造成的 token 浪费。

## 6. 状态应用

推荐使用版本化状态：

```text
读取 base_version
  ↓
验证是否仍为当前版本
  ↓
校验 operations
  ↓
事务性应用
  ↓
写入 event log
  ↓
生成 version 43
  ↓
按策略生成 checkpoint
```

如果用户在 Agent 运行期间已经修改了项目，后端应检测版本冲突，而不是静默覆盖。

## 7. Remotion 数据流

```text
规范化 Timeline State
  ↓
Selector / Adapter
  ↓
Remotion Composition Props
  ↓
Preview 或 Render Job
  ↓
输出视频 / 帧 / 元数据
```

Remotion 不应该直接读取散落在多个 UI 组件里的临时状态。应由统一 Adapter 把领域状态转换为渲染 Props。

## 8. 异步任务

长时间渲染应作为后台任务：

```text
POST /renders
  ↓
创建 render_job
  ↓
Queue / Worker
  ↓
Remotion render
  ↓
上传输出
  ↓
更新状态
  ↓
前端轮询或订阅
```

Agent 对时间线的轻量修改和最终视频渲染是两类不同任务，不应在同一个同步请求中完成。

## 9. 推荐接口

```text
POST /projects/{id}/agent/messages
GET  /projects/{id}/versions/{version}
GET  /projects/{id}/events
POST /projects/{id}/operations/validate
POST /projects/{id}/operations/apply
POST /projects/{id}/renders
GET  /renders/{job_id}
```

## 10. 错误分类

- 用户权限错误。
- 项目版本冲突。
- Agent 输出 schema 错误。
- 操作目标不存在。
- 资产不存在或无权限。
- 时间线约束冲突。
- LLM 超时或限流。
- Remotion 渲染失败。

错误应结构化返回，不能只抛出一段模型文本。

## 11. 核心结论

1. Transfer Agent 应输出可验证操作，而不是无条件覆盖全量 JSON。
2. 后端是权限、版本和操作合法性的可信边界。
3. 时间线领域状态和 Remotion Props 需要 Adapter 隔离。
4. Agent 修改和视频渲染要拆成不同生命周期。
5. 版本、事件和 Snapshot 三者需要分别建模。