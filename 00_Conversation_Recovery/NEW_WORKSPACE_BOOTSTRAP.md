# 新 GPT 工作空间启动说明

本文件用于让新的 GPT 工作空间恢复长期合作方式，但不直接加载所有项目内容。

## 1. 用户与目标

用户是 FangCong，长期关注 AI Agent、MCP、全栈工程、Agent 安全、多智能体可读性、算法和工程面试。

用户希望通过这个 GitHub 仓库延续过去的项目讨论，不在新工作空间中重新从零解释背景。

## 2. 最重要的加载原则

不同项目不得混合恢复。

当用户提出某个项目问题时，只加载该项目的恢复文件和对应知识库：

```text
PROJECT_RECOVERY_INDEX.md
  ↓ 选择一个项目
Projects/<project>/RECOVERY_STATE.md
  ↓ 按文件中的知识库加载清单
仓库根目录下对应的项目知识文档
  ↓ 如涉及实现
检查真实代码仓库和最新分支
```

例如用户讨论 Email MCP：

```text
00_Conversation_Recovery/Projects/05_Email_MCP/RECOVERY_STATE.md
  ↓
05_Email_MCP/README.md
05_Email_MCP/Business_Requirements_and_Enterprise_Scenarios.md
05_Email_MCP/Protocol_Threading_Authentication_and_Permissions.md
05_Email_MCP/Architecture_Tool_List_Skills_and_Testing.md
```

此时不要同时加载 SZU_TK、AAAI 或手撕代码恢复文件。

## 3. 全局文件加载

新工作空间开始时只需先读取：

```text
00_Conversation_Recovery/USER_CONTEXT_AND_COLLABORATION_STYLE.md
00_Conversation_Recovery/PROJECT_RECOVERY_INDEX.md
```

然后等待或识别用户正在讨论哪个项目，再读取该项目独立恢复文件。

## 4. 新助手的工作原则

1. 不要把设计设想说成已经实现。
2. 不要把用户提出的原创思路包装成助手自己的结论。
3. 优先承接用户已经建立的理解，不无理由重讲基础概念。
4. 项目问题要结合真实系统场景、数据流和代码路径。
5. 恢复文件只提供对话状态；技术事实要继续读取对应知识库文档。
6. 如果知识库与最新代码冲突，以最新代码为准，并更新恢复状态。
7. 对未验证判断标记为待代码验证或待实验验证。
8. 不编造功能、测试结果、性能数据或代码路径。

## 5. 回答项目问题的推荐结构

```text
历史背景和已有共同结论
→ 当前代码或资料事实
→ 本次新发现
→ 数据如何流动
→ 核心缺陷
→ 用户原有思路
→ 新增设计建议
→ 验证方式和下一步
```

## 6. 需要避免

- 不要一次读取所有 `Projects/*/RECOVERY_STATE.md`。
- 不要只读恢复状态而跳过项目知识库。
- 不要把不同项目的开放问题写在一起。
- 不要把 Shared Knowledge 当成具体项目状态。
- 不要假设历史文档永远正确。

## 7. 状态更新时间

当前恢复状态主要覆盖截至 2026-07-10 的长期讨论。

后续项目有重大变化时，只更新对应项目的：

```text
00_Conversation_Recovery/Projects/<project>/RECOVERY_STATE.md
```

如果新增项目，再更新：

```text
PROJECT_RECOVERY_INDEX.md
```

普通技术知识仍写入仓库中的原项目目录。