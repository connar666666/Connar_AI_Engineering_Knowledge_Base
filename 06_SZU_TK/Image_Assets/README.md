# SZU_TK / Transfer Agent 图片资源清单

## 当前状态

已在 File Library 中定位到多批 PNG、PPTX 和 PDF 视觉资源。当前尚未把原始二进制导入 GitHub，因此状态为“File Library 可定位”。

## 已定位 PNG 页面

| 原文件名 | 内容 | 建议归档名 | 类型 |
|---|---|---|---|
| `slide-01.png` / `slide-1.png` | 项目封面、爆款视频结构迁移平台 | `slides/szutk_product_cover.png` | PPT 导出图 |
| `slide-02.png` / `slide-2.png` | 业务价值：结构资产、自动化迁移、编辑器落地 | `slides/szutk_business_value.png` | PPT 导出图 |
| `slide-03.png` | 用户任务流程或产品定位 | `slides/szutk_user_flow_or_positioning.png` | PPT 导出图 |
| `slide-05.png` | Analysis / Transfer 技术总览或 Transfer Agent 编排器 | `diagrams/szutk_analysis_transfer_overview.png` | 架构图 |
| `slide-07.png` | 素材缺口闭环 | `diagrams/szutk_material_gap_loop.png` | 流程图 |
| `slide-12.png` | 素材缺口上报与继续任务 | `diagrams/szutk_gap_report_and_resume.png` | 流程图 |
| `slide-13.png` | `commit_to_editor` 与 EditorEditPlan | `diagrams/szutk_commit_to_editor.png` | 数据流图 |
| `slide-14.png` | 实际演示、启动前后端、真实操作 | `slides/szutk_live_demo_flow.png` | 演示页 |

注意：历史文件中存在同名但内容不同的 `slide-1.png`、`slide-2.png`、`slide-03.png` 和 `slide-05.png`，导入前必须通过画面内容去重，不能只按文件名覆盖。

## 已定位完整演示稿

- `SZUTK_Transfer_Agent_核心介绍.pptx`
- `SZUTK_Analysis_Transfer_技术实现比赛版_字号加深版.pptx`
- `SZUTK_Analysis_Transfer_技术实现比赛版_颜色修复_实际演示版.pptx`
- `SZUTK_Analysis_Transfer_技术实现比赛版_少字备注版.pptx`
- `SZUTK_Transfer_Agent_亮色产品展示版_导航栏放大_实际演示版.pptx`
- 对应多个 PDF 导出版本。

建议把最终确认版本放在：

```text
source/presentations/
```

页面导出图放在：

```text
slides/
```

## 需要区分的资源类型

### Image 2 / AI 生成图片

例如抽象架构插画、封面视觉和流程示意。如果找到原始生成图，放入：

```text
generated/
```

并记录生成提示词、生成日期和使用场景。

### PPT 页面导出

PPT 页面本身不应被错误标成 Image 2 原图，应放入：

```text
slides/
```

### 产品真实截图

真实 Web 页面或编辑器截图放入：

```text
screenshots/
```

## 导入后的清单字段

每张图片应在本文件补充：

```text
文件路径
来源对话或源文件
图片类型
生成或导出时间
使用场景
是否最终版
是否存在可编辑源文件
```

## 当前缺口

当前工具无法从 File Library 提取图片原始二进制，因此这里只完成资源识别和归档规划。原件导出后应直接提交到本目录，不应重新截图压缩。