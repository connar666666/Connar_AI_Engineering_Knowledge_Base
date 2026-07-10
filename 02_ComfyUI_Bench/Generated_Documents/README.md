# ComfyUI WorkBench / FrameWeave：历史生成文档归档

## 权威成品

| 历史交付主题 | 权威文件 | 状态 |
|---|---|---|
| 项目整体架构说明 | `../Architecture_Overview.md` | 权威成品 |
| 后端 FastAPI、Redis、SQL、OSS、Worker 理解 | `../Backend_Understanding.md` | 权威成品 |
| 前端模块与页面设计 | `../Frontend_Design.md` | 权威成品 |
| 数据库结构与权限方向 | `../Database_Design.md` | 权威成品 |
| Project / Canvas 权限与邀请系统 | `../Permission_and_Invite_System.md` | 权威成品 |
| Docker、任务队列和部署 | `../Deployment_and_DevOps.md` | 权威成品 |

## 历史重建成品

| 文档 | 说明 |
|---|---|
| `Database_Schema_V3_and_Invitation_System_REBUILT.md` | 重建用户曾要求的完整 V3 数据库、邀请与非对称权限设计 |

## 历史请求来源

用户曾明确要求：

- 给出完整数据库 V3 Schema。
- 增加 Project 与 Canvas 的邀请记录。
- Project 权限可向下复用，但 Canvas 权限不能反向获得 Project 权限。
- 前端需要邀请弹窗、用户搜索和首页邀请通知。
- 结合最新代码给出前后端修改方案。

## 仍待代码验证

- 最新仓库是否已经实现 V3 Schema。
- 邀请 API、前端弹窗和首页信封是否已经落地。
- 真实索引、迁移文件和权限 SQL。

历史重建版是设计成品，不代表当前代码已经实现。