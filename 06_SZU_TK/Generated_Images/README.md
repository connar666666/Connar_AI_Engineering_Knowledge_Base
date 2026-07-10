# SZU_TK / Transfer Agent：历史图片归档

## 当前状态

`FOUND_IN_FILE_LIBRARY + BINARY_PENDING`

当前已经在 File Library 中确认存在一批真实历史图片和整套 PPT/PDF 视觉资源，但原始二进制文件尚未通过当前连接器传入 GitHub。

## 已确认资源

### 产品与业务页面

- `slide-01.png`：SZUTK Transfer Agent 产品封面。
- `slide-02.png`：爆款视频难复用的业务痛点。
- `slide-03.png`：产品定位与“爆款参考—用户素材—Agent 编排”。
- `slide-04.png`：Transfer Agent 产品能力。
- `slide-05.png`：Transfer Agent 对话式编排器架构图。
- `slide-06.png`：一次迁移任务完整路径。
- `slide-07.png`：素材缺口闭环。
- `slide-08.png`：可编辑多轨时间线。

### 技术实现与演示页面

- `slide-09.png`：agent_view、state-store、editor plan 与工具机制。
- `slide-10.png`：当前源码与旧 README 差异。
- `slide-11.png`：网页端完整演示流程。
- `slide-12.png`：代码启动、Web 实现和最终验证。
- `slide-14.png`：切换代码并跑通真实链路。
- `slide-8.png`：Analysis 多 Agent 分层推理。
- `slide-9.png`：TemplateTransferAgentSession 与 17 个 tools。
- `slide-12.png`：素材缺口报告与继续任务流程。

### 汇总资源

- `nav_big_montage.png`：完整幻灯片拼图。
- `SZUTK_Transfer_Agent_核心介绍.pptx`
- `SZUTK_Transfer_Agent_源码版核心介绍.pptx`
- `SZUTK_Transfer_Agent_业务产品展示版.pptx`
- `SZUTK_Transfer_Agent_亮色产品展示版_导航栏放大_实际演示版.pdf`

## 建议目标文件名

```text
assets/
├── 2026-06-22_szutk-product-cover.png
├── 2026-06-22_business-pain-points.png
├── 2026-06-22_product-positioning.png
├── 2026-06-22_transfer-agent-capabilities.png
├── 2026-06-22_transfer-agent-orchestrator.png
├── 2026-06-22-migration-task-flow.png
├── 2026-06-22-material-gap-loop.png
├── 2026-06-22-editable-timeline.png
├── 2026-06-22-agent-view-state-tools.png
├── 2026-06-22-source-code-vs-readme.png
├── 2026-06-22-live-demo-flow.png
├── 2026-06-22-analysis-transfer-workflow.png
├── 2026-06-22-analysis-multi-agent.png
├── 2026-06-22-transfer-agent-runtime-tools.png
└── 2026-06-22-slide-montage.png
```

## 与知识文档对应

```text
Backend_Architecture_and_Data_Flow.md
Transfer_Agent_Context_and_Snapshot_Design.md
Frontend_Remotion_and_Template_Refactor.md
Generated_Documents/README.md
```

## 待执行

取得原始文件后：

1. 按语义重命名。
2. 去重同一页的多个版本。
3. 保存最高质量版本。
4. 写入 `IMAGE_ASSET_MANIFEST.md`。
5. 在相关 Markdown 中嵌入相对路径。

## 重要说明

当前 README 是资源清单，不代表 PNG/PPTX/PDF 已经写入仓库。