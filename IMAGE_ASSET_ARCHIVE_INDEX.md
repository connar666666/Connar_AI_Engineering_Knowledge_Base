# 历史图片资源归档索引

> 目标：把历史对话中通过 Image 2、图片生成、图示制作、PPT 页面导出或截图产生的视觉资源按项目归档。

## 1. 目录规范

每个项目使用：

```text
<Project>/Image_Assets/
├── README.md
├── generated/      AI 生成图片
├── diagrams/       架构图、流程图、状态机
├── screenshots/    产品截图、代码截图、终端截图
├── slides/         PPT 页面导出图
└── source/         可编辑源文件或原始素材
```

普通对话产生、无法归属某个项目的图片放在：

```text
Shared_Knowledge/Image_Assets/
```

## 2. 资产状态

- **原图已归档**：PNG/JPG/WebP/SVG 等二进制文件已经提交到仓库。
- **源文件已归档**：PPTX、PDF、SVG、Mermaid 等可编辑或可重新导出的来源已经提交。
- **File Library 可定位**：能在历史文件库中找到文件，但当前尚未导出到 GitHub。
- **仅对话引用**：知道生成过，但目前没有可访问原件。
- **可重建**：可根据明确提示词和设计重新生成，但不等同于原图。

## 3. 当前发现

目前已在 File Library 中明确定位到一批 SZU_TK / Transfer Agent 视觉资产，包括：

- 产品首页和项目封面。
- 业务价值页面。
- Analysis 与 Transfer 技术总览。
- Transfer Agent 对话式编排器图。
- 素材缺口闭环图。
- `commit_to_editor` 多轨编辑器输入输出图。
- 实际演示与代码启动流程页。
- 多个完整 PPTX 与 PDF 版本。

这些文件当前属于“File Library 可定位”，尚未作为原始二进制提交到 GitHub。

## 4. 当前工具限制

当前 File Library 检索可以识别和查看历史图片，但无法把历史图片的原始二进制直接传递给 GitHub Connector。

因此本轮先完成：

1. 项目归属。
2. 文件名和内容清点。
3. 目录规范。
4. 待导入清单。
5. 去重规则。

真正提交 PNG/JPG/PPTX/PDF 原件需要：

- 从 File Library 或旧对话下载后重新上传；或
- 提供原始本地文件；或
- 对可接受重建的图片重新生成。

## 5. 去重与命名

图片文件建议使用语义化名称，不保留大量重复的 `slide-1.png`：

```text
szutk_product_cover.png
szutk_business_value.png
szutk_analysis_transfer_overview.png
szutk_transfer_agent_orchestrator.png
szutk_material_gap_loop.png
szutk_commit_to_editor.png
```

对于多个版本，使用：

```text
<name>_v1.png
<name>_v2.png
<name>_final.png
```

不要仅根据文件创建时间判断哪个是最终版本，应结合内容和对应 PPT/PDF 版本确认。