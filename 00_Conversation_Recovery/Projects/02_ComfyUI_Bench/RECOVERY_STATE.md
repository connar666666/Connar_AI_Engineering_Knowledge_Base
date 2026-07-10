# ComfyUI WorkBench / FrameWeave 对话恢复状态

## 项目目标
构建围绕 Project、Canvas、资产、生成任务、权限、邀请和视频工作流的全栈平台。

## 对话历史关键点
- **用户提出并纠正早期设计**：Project 权限可向下复用给 Canvas，但只被邀请到某个 Canvas 的用户不能自动获得整个 Project 权限。
- **用户提出**：Project 和 Canvas 都需要邀请用户，首页需要邀请通知入口。
- **助手补充**：Project Member、Canvas Member、Project Invitation、Canvas Invitation 应独立建模。
- **共同结论**：权限继承是非对称的，邀请记录必须保留状态、来源和审计。

## 当前共同结论
- Project 是上层容器，Canvas 是具体工作区。
- Project 成员可访问下属 Canvas；Canvas 独立成员权限不反向扩张。
- SQL 保存长期结构化数据，Redis 管短期状态和队列，对象存储保存大文件。
- 前端邀请弹窗、后端用户搜索、邀请状态和首页通知需要形成完整闭环。

## 已讨论内容
- 数据库 V3 和邀请系统。
- FastAPI、Redis、SQL、Docker、对象存储和 Worker。
- Project / Canvas 前端交互。
- Remotion、视频素材和人物一致性方向。

## 待验证
- 最新代码中的数据库 Schema 和迁移状态。
- Project / Canvas 邀请前后端是否已经实现。
- 首页信封通知是否存在。
- 真实权限检查 SQL 和 API。
- Remotion 胶水代码、渲染链路和人物一致性方案。

## 知识库加载清单

```text
02_ComfyUI_Bench/README.md
02_ComfyUI_Bench/Architecture_Overview.md
02_ComfyUI_Bench/Database_Design.md
02_ComfyUI_Bench/Permission_and_Invite_System.md
02_ComfyUI_Bench/Backend_Understanding.md
02_ComfyUI_Bench/Frontend_Design.md
02_ComfyUI_Bench/Deployment_and_DevOps.md
```

## 恢复后继续位置
根据用户指定的最新仓库和分支检查数据库、邀请和前端代码，把知识库中的设计结论与真实实现逐项对齐。