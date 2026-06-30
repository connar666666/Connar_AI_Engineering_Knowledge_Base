# ComfyUI_Bench：整体架构理解

## 1. 项目整体定位

ComfyUI_Bench / WorkBench 可以理解为一个围绕 AI 生成工作流构建的全栈平台。它不是单纯调用 ComfyUI，而是需要管理用户、项目、画布、资产、任务、输出结果和权限。

## 2. 总体链路

```text
Browser Frontend
  ↓ HTTP / WebSocket
Nginx / Gateway
  ↓
FastAPI Backend
  ├── SQL Database
  ├── Redis
  ├── OSS Object Storage
  ├── Worker / Queue
  └── ComfyUI Runtime
```

## 3. 核心模块

### 3.1 用户系统

用户系统负责：

- 登录。
- 当前用户识别。
- 用户搜索。
- 邀请用户加入 Project 或 Canvas。
- 首页右上角邀请通知。

### 3.2 Project

Project 是最高层级的组织单位。它下面可以包含多个 Canvas。

Project 权限可以作为 Canvas 的默认权限来源，但 Canvas 权限不应该反向影响 Project。

### 3.3 Canvas

Canvas 是具体创作空间。一个 Project 下可以有多个 Canvas。

Canvas 可以继承 Project 成员，也可以有自己额外的权限规则。

### 3.4 Canvas Version / Detail

Canvas 需要版本系统，用于记录不同阶段的画布状态、编辑历史和生成配置。

### 3.5 Canvas Output

Canvas Output 保存生成结果，通常关联 OSS Object。

### 3.6 Asset Library

项目资产库用于管理用户上传的图片、视频、音频、模型文件、参考素材等。

## 4. 数据流

```text
用户上传素材
  ↓
OSS 保存原始文件
  ↓
Asset 表记录元数据
  ↓
Canvas 引用 Asset
  ↓
后端提交 ComfyUI 任务
  ↓
Redis/Queue 记录任务状态
  ↓
Worker 执行生成
  ↓
结果保存到 OSS
  ↓
CanvasOutput 记录结果
```

## 5. 工程重点

这个项目要重点理解的不是单个技术，而是技术之间如何配合：

- FastAPI 处理 HTTP 请求。
- SQL 保存长期结构化数据。
- Redis 保存短期任务状态、缓存、队列状态。
- OSS 保存大文件。
- Worker 执行耗时生成任务。
- 前端负责展示 Project、Canvas、邀请、任务状态和结果。

