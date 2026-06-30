# ComfyUI_Bench：权限系统与邀请系统

## 1. 核心结论

Project 和 Canvas 的权限关系是非对称的：

```text
Project 权限可以向下影响 Canvas；
Canvas 权限不能向上影响 Project。
```

这意味着：

- Project 成员可以默认访问 Project 下的 Canvas。
- Canvas 可以额外邀请用户参与。
- 但 Canvas 成员不应该自动获得整个 Project 的权限。

## 2. 为什么不能只用 project_members

如果只用 `project_members`，那么所有协作者都会变成项目级成员，这会导致权限过大。

例如：

```text
一个外部设计师只需要参与某个 Canvas，
但不应该看到整个 Project 下的其他 Canvas 和素材库。
```

因此需要：

```text
project_members
canvas_members
```

两套关系。

## 3. 为什么需要 invite 表

邀请不是简单地把用户插入 member 表。邀请有一个过程：

```text
发送邀请
  ↓
pending
  ↓
用户接受 / 拒绝
  ↓
accepted / declined
  ↓
写入 member 表
```

如果没有 invite 表，就无法实现：

- 首页信封通知。
- 查看待处理邀请。
- 拒绝邀请。
- 防止重复邀请。
- 记录谁邀请了谁。
- 审计历史。

## 4. Project 邀请流程

```text
Project Owner / Admin 点击邀请
  ↓
搜索用户
  ↓
选择角色
  ↓
创建 project_invitation
  ↓
被邀请者首页信封出现通知
  ↓
被邀请者接受
  ↓
写入 project_members
  ↓
邀请状态变为 accepted
```

## 5. Canvas 邀请流程

```text
Canvas Owner / 有权限成员点击邀请
  ↓
搜索用户
  ↓
选择角色
  ↓
创建 canvas_invitation
  ↓
被邀请者首页信封出现通知
  ↓
被邀请者接受
  ↓
写入 canvas_members
  ↓
邀请状态变为 accepted
```

## 6. 前端需要的功能

### 邀请弹窗

需要支持：

- 搜索用户名。
- 搜索邮箱。
- 显示候选用户。
- 选择角色。
- 显示已邀请/已加入状态。

### 信封图标

首页右上角需要：

- 显示待处理邀请数量。
- 点击打开邀请列表。
- 接受邀请。
- 拒绝邀请。

## 7. 后端 API 建议

```text
GET  /users/search?q=
POST /projects/{project_id}/invites
POST /canvas/{canvas_id}/invites
GET  /me/invites
POST /invites/{invite_id}/accept
POST /invites/{invite_id}/decline
```

## 8. 权限角色建议

```text
owner
admin
editor
viewer
```

Project 和 Canvas 可以共享角色名，但权限含义要分别定义。

## 9. 关键边界条件

1. 被邀请用户已经是成员时，不应重复邀请。
2. pending 邀请不能重复创建。
3. 只有有权限的人才能邀请。
4. Project 被删除时，其邀请应失效。
5. Canvas 被删除时，其邀请应失效。
6. Canvas 邀请接受后，不应写入 project_members。

