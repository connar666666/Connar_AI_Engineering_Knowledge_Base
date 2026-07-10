# SZU_TK：前端、Remotion 与模板重构

## 1. 当前问题类型

对话中提到的前端问题可以分成三类：

1. Remotion 相关代码散落在 UI、状态管理和数据转换之间，形成胶水代码。
2. 页面展示的四个模板仍然是早期 Mock 占位内容，并不是真实分析或生成结果。
3. Transfer Agent 的状态组装、UI 状态和渲染 Props 可能耦合，导致修改困难。

## 2. 推荐分层

```text
Domain State
  ↓ selectors
View Model
  ↓ adapter
Remotion Props
  ↓
Composition / Preview / Render
```

### Domain State

描述项目真实数据：

- timeline。
- tracks。
- clips。
- assets。
- transitions。
- captions。
- audio。

### View Model

用于 UI 展示：

- 当前选中项。
- 面板展开状态。
- 缩放比例。
- 拖拽中的临时状态。

### Remotion Props

只包含渲染所需的稳定输入，不应混入弹窗、选中状态或临时 UI 标记。

## 3. 胶水代码的典型缺陷

- 同一字段在多个组件中重复转换。
- 组件既请求数据又修改领域状态又生成 Remotion Props。
- 大量 `any`、可选链和兜底值掩盖数据模型不一致。
- Mock 模板与真实模板使用同一接口，但字段语义不同。
- 时间单位混用：秒、毫秒、frame。
- 资产 URL、OSS Key 和本地预览 URL 混用。
- Agent 输出直接进入渲染层，没有 schema 校验。

## 4. 时间单位统一

内部领域模型建议统一使用毫秒，Remotion 边界再转换为 frame：

```text
frame = round(time_ms / 1000 * fps)
```

Adapter 负责：

- `start_ms` → `from` frame。
- `duration_ms` → `durationInFrames`。
- 处理 fps。
- 防止负数、重叠和越界。

不要在每个 React 组件中重复计算。

## 5. 模板数据模型

早期四个 Mock 模板应和真实模板区分。

推荐：

```text
template_definitions
- template_id
- name
- version
- source: builtin | analyzed | generated
- schema_version
- thumbnail
- composition_id
- default_props
- constraints
```

### Builtin Template

仓库内固定定义，可用于演示和兜底。

### Analyzed Template

从素材或视频分析得到，应保存分析任务、模型版本和置信度。

### Generated Template

由 Agent 生成，应保存输入需求、生成版本、校验状态和可编辑结构。

前端不能继续把 Mock 数组当成真实分析结果。

## 6. 推荐前端模块

```text
features/editor/
features/transfer-agent/
features/templates/
features/remotion-preview/
features/render-jobs/
entities/project/
entities/timeline/
entities/asset/
shared/api/
shared/schema/
```

可能的核心文件：

```text
timeline.schema.ts
timeline.selectors.ts
remotion.adapter.ts
template.schema.ts
agent-operation.schema.ts
render-job.api.ts
```

实际路径需要结合仓库现状调整。

## 7. Transfer Agent 前端流程

```text
用户输入
  ↓
读取 current project version
  ↓
Context Builder 选择必要状态
  ↓
POST agent message
  ↓
展示 plan / operations preview
  ↓
用户确认或自动应用低风险操作
  ↓
后端返回 new_version + diff
  ↓
前端刷新领域状态
  ↓
Adapter 生成新的 Remotion Props
```

不要让聊天组件自己维护一份独立时间线副本。

## 8. Diff 驱动更新

后端返回：

```json
{
  "base_version": 42,
  "new_version": 43,
  "diff": {
    "added": [],
    "updated": [],
    "removed": []
  }
}
```

前端可以：

- 高亮变化。
- 展示撤销入口。
- 避免重新传输全部历史。
- 检测版本冲突。

## 9. Remotion 原始素材输入

Remotion 可以使用原始图片、视频、音频和字体等素材，关键是让素材成为可解析的 URL、静态资源或受控文件引用。

推荐资产流程：

```text
上传原始素材
  ↓
保存 Asset 元数据和对象存储引用
  ↓
编辑器引用 asset_id
  ↓
渲染前解析为签名 URL / 本地文件
  ↓
Remotion Composition 使用
```

不要把大文件本体直接放入 Agent JSON。

## 10. 删除无效内容的标准

可以删除：

- 已无引用的 Mock 数据。
- 重复转换函数。
- 与当前领域 schema 不一致的旧类型。
- 仅为临时演示保留且已有真实替代的数据。
- 被 Adapter 取代的组件内 frame 计算。

暂时不要直接删除：

- 仍被路由或演示页面引用的模板。
- 仍作为迁移兼容层使用的字段。
- 没有测试覆盖的关键转换逻辑。

删除前应通过代码搜索和测试确认引用关系。

## 11. 测试重点

- Timeline schema 校验。
- 时间到 frame 的转换。
- Clip 排序和重叠处理。
- 资产 URL 解析。
- Template source 区分。
- Agent operation 到 timeline diff。
- 同一状态生成稳定 Remotion Props。
- Preview 与最终 Render 使用相同 Adapter。

## 12. 最终结论

重构重点不是把文件拆小，而是建立清晰边界：

```text
真实领域状态
≠ UI 临时状态
≠ Agent 上下文
≠ Remotion 渲染输入
```

四者通过明确 schema、selector 和 adapter 连接，才能避免新的胶水代码继续增长。