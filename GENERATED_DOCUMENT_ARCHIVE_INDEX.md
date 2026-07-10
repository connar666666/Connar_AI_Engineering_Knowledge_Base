# 历史生成文档清点与成品归档索引

> 清点时间：2026-07-10  
> 范围：根据当前仓库、已合并 PR、当前可访问的对话上下文和长期项目记忆进行整理。

## 1. 归档目标

本层专门保存或登记用户在历史对话中明确要求生成的完整 Markdown、设计稿、说明书、邮件、复盘文档和可直接使用的最终稿。

它与其他两层不同：

```text
项目知识库
= 面向理解与长期维护的技术沉淀

Conversation Recovery
= 面向新 GPT 工作空间的项目状态恢复

Generated Documents Archive
= 历史上已经生成过、可以直接阅读或使用的完整成品
```

## 2. 归档状态

- **权威成品**：完整内容已经存在于仓库，归档索引直接指向该文件，不重复复制。
- **历史重建版**：历史上明确生成过，但当前无法取得当时逐字原文；根据对话、现有知识文档和已确认结论重建。
- **待补原件**：知道曾生成，但依赖当前无法访问的附件、逐字稿或旧工作区原文，不能诚实重建。
- **非成品对话**：只是即时问答或零散解释，没有形成完整可交付文档，不强行归档。

## 3. 项目级归档入口

```text
01_AAAI_Legibility_Committee/Generated_Documents/README.md
02_ComfyUI_Bench/Generated_Documents/README.md
03_OpenAI_Agent_Attack/Generated_Documents/README.md
04_AgentOS_OSAgent/Generated_Documents/README.md
05_Email_MCP/Generated_Documents/README.md
06_SZU_TK/Generated_Documents/README.md
07_Hand_Coding/Generated_Documents/README.md
08_Internship/Generated_Documents/README.md
Engineering/Generated_Documents/README.md
```

## 4. 普通对话成品入口

不属于单一项目的通用技术文档统一放在：

```text
Shared_Knowledge/Generated_Documents/
```

主要包括：

- FastAPI、HTTP、Redis、SQL 全链路解释。
- Rubric、Harness 和 Agent 评测。
- MCP Server、Agent Loop 与传输方式。
- 装饰器和 FastAPI 路由。
- Docker 镜像与镜像仓库。
- LaTeX 算法排版模板。
- 其他跨项目复用的完整说明文档。

## 5. 不重复复制原则

如果完整成品已经位于项目根目录，例如：

```text
05_Email_MCP/Mail_Server_Questionnaire.md
06_SZU_TK/Transfer_Agent_Context_and_Snapshot_Design.md
```

则 `Generated_Documents/README.md` 将它登记为权威成品，而不会复制一份相同内容。

只有以下情况新增文件：

1. 历史成品当前缺失。
2. 多份知识文档需要合并为一个可直接提交或发送的最终稿。
3. 需要明确区分“历史重建版”和普通知识笔记。

## 6. 可信度限制

当前无法直接读取所有旧工作区聊天全文和部分已上传附件，因此：

- 不声称所有文档都是当时的逐字原版。
- 不根据记忆伪造面试原回答、实验结果、代码路径或性能数据。
- 无法恢复的原件会标记为“待补原件”。
- 历史重建版文件名会包含 `REBUILT`，并在文件开头说明来源。

## 7. 新工作空间使用方式

当需要恢复某个项目过去交付过的文档时：

```text
选择项目
→ 读取项目 Generated_Documents/README.md
→ 打开权威成品或历史重建版
→ 再读取 Conversation Recovery 了解当前状态
→ 涉及实现时检查最新代码
```

不要把历史成品、知识总结和当前代码状态混为同一事实来源。