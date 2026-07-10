# 历史生成图片与视觉资源归档索引

> 清点时间：2026-07-10

## 1. 目标

图片、架构图、流程图、PPT 导出页、演示截图和 Image 模型生成图都属于项目资源，不应只保留在聊天记录中。

统一结构：

```text
<Project>/Generated_Images/
├── README.md
├── IMAGE_ASSET_MANIFEST.md   # 有实际资源时
└── assets/                   # 原始 PNG/JPG/WebP/GIF/SVG
```

普通技术对话产生的图片统一放在：

```text
Shared_Knowledge/Generated_Images/
```

## 2. 当前可执行范围

当前已经从 File Library 检索到一批历史视觉资源，重点集中在 SZU_TK / Transfer Agent，包括：

- 产品封面和业务价值页。
- Analysis / Transfer 流程图。
- Transfer Agent 对话式编排器架构图。
- 素材缺口闭环。
- 多 Agent 分层分析。
- Agent Runtime 与 17 个工具。
- 多轨编辑器时间线。
- 实际演示流程。
- 完整 PPT、PDF 和 montage 资源。

## 3. 当前技术限制

File Library 当前只向本会话提供图片引用、标题、预览和语义信息，没有向 GitHub 上传接口暴露原始二进制文件或可下载路径。

因此当前归档分两步：

```text
第一步：建立项目目录、资源清单、目标文件名和来源映射
第二步：取得原始 PNG/JPG/PPTX/PDF 后写入 assets/
```

未取得二进制文件前，不能声称图片已经真正上传到 GitHub。

## 4. 归档状态

- **FOUND_IN_FILE_LIBRARY**：已找到真实历史资源引用。
- **BINARY_PENDING**：可预览，但原始字节尚未传入 GitHub。
- **UPLOADED**：原始文件已经写入仓库。
- **REBUILT**：根据旧图内容重新生成，不是原始图片。
- **NOT_FOUND_YET**：当前检索未找到，不代表历史上不存在。

## 5. 项目入口

```text
01_AAAI_Legibility_Committee/Generated_Images/README.md
02_ComfyUI_Bench/Generated_Images/README.md
03_OpenAI_Agent_Attack/Generated_Images/README.md
04_AgentOS_OSAgent/Generated_Images/README.md
05_Email_MCP/Generated_Images/README.md
06_SZU_TK/Generated_Images/README.md
07_Hand_Coding/Generated_Images/README.md
08_Internship/Generated_Images/README.md
Engineering/Generated_Images/README.md
Shared_Knowledge/Generated_Images/README.md
```

## 6. 文件命名规范

```text
YYYY-MM-DD_<topic>_<version>.<ext>
```

示例：

```text
2026-06-22_transfer-agent-product-cover_v1.png
2026-06-22_analysis-transfer-workflow_v1.png
2026-06-22_agent-runtime-tools_v1.png
```

同名 `slide-01.png` 必须重命名，避免不同 PPT 版本互相覆盖。

## 7. 后续更新规则

每次生成重要图片时，应同时保存：

- 原始图片文件。
- 图片用途和所属项目。
- 生成日期。
- Prompt 或设计说明（不含敏感信息）。
- 对应文档或 PPT。
- 是否为原图、修改版或重建版。