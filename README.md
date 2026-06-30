# Connar_AI_Engineering_Knowledge_Base

> 这是从 ChatGPT Business 工作空间的长期对话中整理出来的个人 AI 工程知识库初版。  
> 目标仓库：https://github.com/connar666666/Connar_AI_Engineering_Knowledge_Base

这个知识库不是简单的聊天记录备份，而是把已经讨论过的重要内容按照项目拆分成多个 Markdown 文档，方便手动上传到 GitHub，并在后续持续补充。

## 当前项目结构

```text
Connar_AI_Engineering_Knowledge_Base/
├── README.md
├── UPLOAD_TO_GITHUB.md
├── 01_AAAI_Legibility_Committee/
├── 02_ComfyUI_Bench/
├── 03_OpenAI_Agent_Attack/
├── 04_AgentOS_OSAgent/
├── Shared_Knowledge/
└── Engineering/
```

## 四个核心项目

### 01_AAAI_Legibility_Committee

用于整理 AAAI 可读性项目，包括 LBF、规则发现、奖励塑形、实验启动脚本、论文修改计划等。

### 02_ComfyUI_Bench

用于整理 ComfyUI/Workbench/FrameWeave 相关工程项目，包括数据库设计、前后端理解、权限系统、邀请系统、FastAPI、Redis、SQL、Docker、任务队列和部署。

> 说明：之前对话中出现过 `ComfyUI_WorkBench` 这个名称；现在按照你的最新要求，目录统一使用 `02_ComfyUI_Bench`。文档内部仍会说明它覆盖 WorkBench / FrameWeave 的工程内容。

### 03_OpenAI_Agent_Attack

用于整理 OpenAI Agent Attack 比赛项目，包括 prompt 搜索、变异、向量空间、攻击框架、Harness、Rubric 和评测机制。

### 04_AgentOS_OSAgent

用于整理 AgentOS 与 OS Agent 方向，包括两者区别、未来 OS 形态、MCP、记忆系统、调度系统、多 Agent 协作等。

## 共享知识与工程记录

`Shared_Knowledge/` 保存跨项目复用的基础知识，例如 FastAPI、Redis、SQL、LLM、RAG、Embedding、RLHF、GRPO 等。

`Engineering/` 保存工具、环境、硬件和开发经验，例如 Windows 内存升级、Git、GitHub、Codex、Claude Code、WSL、Docker 等。

## 使用方式

你可以直接把整个文件夹上传到 GitHub 仓库中，也可以先上传四个项目目录，再逐步补充 Shared_Knowledge 和 Engineering。

推荐提交信息：

```bash
git add .
git commit -m "Initialize project-based AI engineering knowledge base"
git push
```

