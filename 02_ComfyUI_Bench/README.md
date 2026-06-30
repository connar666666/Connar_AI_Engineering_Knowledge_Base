# 02_ComfyUI_Bench

这个目录用于整理 ComfyUI_Bench / ComfyUI_WorkBench / FrameWeave 相关工程项目。

## 项目定位

这个项目不是单纯的 ComfyUI 使用笔记，而是一个完整 AI 工程项目，覆盖：

- AI 工作流平台
- 前端页面设计
- 后端 FastAPI 服务
- Redis 任务状态与缓存
- SQL 数据库设计
- Project / Canvas 权限系统
- 用户邀请系统
- OSS 对象存储
- Asset Library
- Docker 与部署

## 当前文档

```text
02_ComfyUI_Bench/
├── README.md
├── Architecture_Overview.md
├── Backend_Understanding.md
├── Frontend_Design.md
├── Database_Design.md
├── Permission_and_Invite_System.md
└── Deployment_and_DevOps.md
```

## 核心理解

这个项目的重点不是“怎么用 ComfyUI 生成图片”，而是如何围绕 ComfyUI 构建一个真正的 Web 产品：

```text
用户
  ↓
前端页面
  ↓
FastAPI 后端
  ↓
数据库 / Redis / OSS
  ↓
ComfyUI / Worker
  ↓
生成结果
  ↓
回到用户的 Project / Canvas
```

## 后续要继续整理

- 数据库 V3 完整 schema。
- Project 权限和 Canvas 权限的继承/覆盖关系。
- 邀请弹窗的前端交互设计。
- 后端用户搜索与邀请 API。
- 首页右上角信封图标和邀请通知逻辑。
- ComfyUI 任务队列和结果回写流程。

