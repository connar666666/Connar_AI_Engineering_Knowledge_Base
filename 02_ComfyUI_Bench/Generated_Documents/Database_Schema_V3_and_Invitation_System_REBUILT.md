# FrameWeave / ComfyUI WorkBench 数据库 V3 与邀请系统

> 状态：历史重建版。根据过去关于 Project、Canvas、邀请、权限继承和资产管理的多轮讨论重建，不代表当前代码已实现。

## 1. 核心权限原则

```text
Project 权限可以向下覆盖 Project 下的 Canvas
Canvas 独立权限不能反向获得整个 Project 权限
```

因此必须分别建模：

- `project_members`
- `canvas_members`
- `project_invitations`
- `canvas_invitations`

邀请记录不能只在接受后变成成员，因为还需要支持 pending、declined、expired、cancelled、通知与审计。

## 2. DBML Schema

```dbml
Table users {
  id uuid [pk]
  username varchar [not null, unique]
  email varchar [not null, unique]
  avatar_url varchar
  status varchar [not null, default: 'active']
  created_at timestamp [not null]
  updated_at timestamp [not null]
}

Table projects {
  id uuid [pk]
  owner_id uuid [not null, ref: > users.id]
  name varchar [not null]
  description text
  visibility varchar [not null, default: 'private']
  created_at timestamp [not null]
  updated_at timestamp [not null]
  deleted_at timestamp

  indexes {
    owner_id
    (owner_id, updated_at)
  }
}

Table project_members {
  id uuid [pk]
  project_id uuid [not null, ref: > projects.id]
  user_id uuid [not null, ref: > users.id]
  role varchar [not null]
  invited_by uuid [ref: > users.id]
  joined_at timestamp [not null]
  created_at timestamp [not null]
  updated_at timestamp [not null]

  indexes {
    (project_id, user_id) [unique]
    user_id
  }
}

Table project_invitations {
  id uuid [pk]
  project_id uuid [not null, ref: > projects.id]
  inviter_id uuid [not null, ref: > users.id]
  invitee_id uuid [not null, ref: > users.id]
  role varchar [not null]
  status varchar [not null, default: 'pending']
  message text
  expires_at timestamp
  responded_at timestamp
  created_at timestamp [not null]
  updated_at timestamp [not null]

  indexes {
    (project_id, invitee_id, status)
    (invitee_id, status, created_at)
  }
}

Table canvases {
  id uuid [pk]
  project_id uuid [not null, ref: > projects.id]
  owner_id uuid [not null, ref: > users.id]
  name varchar [not null]
  description text
  current_version_id uuid
  created_at timestamp [not null]
  updated_at timestamp [not null]
  deleted_at timestamp

  indexes {
    project_id
    (project_id, updated_at)
  }
}

Table canvas_members {
  id uuid [pk]
  canvas_id uuid [not null, ref: > canvases.id]
  user_id uuid [not null, ref: > users.id]
  role varchar [not null]
  invited_by uuid [ref: > users.id]
  joined_at timestamp [not null]
  created_at timestamp [not null]
  updated_at timestamp [not null]

  indexes {
    (canvas_id, user_id) [unique]
    user_id
  }
}

Table canvas_invitations {
  id uuid [pk]
  canvas_id uuid [not null, ref: > canvases.id]
  inviter_id uuid [not null, ref: > users.id]
  invitee_id uuid [not null, ref: > users.id]
  role varchar [not null]
  status varchar [not null, default: 'pending']
  message text
  expires_at timestamp
  responded_at timestamp
  created_at timestamp [not null]
  updated_at timestamp [not null]

  indexes {
    (canvas_id, invitee_id, status)
    (invitee_id, status, created_at)
  }
}

Table canvas_versions {
  id uuid [pk]
  canvas_id uuid [not null, ref: > canvases.id]
  version_number int [not null]
  created_by uuid [not null, ref: > users.id]
  parent_version_id uuid [ref: > canvas_versions.id]
  note text
  created_at timestamp [not null]

  indexes {
    (canvas_id, version_number) [unique]
    (canvas_id, created_at)
  }
}

Table canvas_details {
  id uuid [pk]
  canvas_version_id uuid [not null, unique, ref: > canvas_versions.id]
  schema_version varchar [not null]
  data_json jsonb [not null]
  created_at timestamp [not null]
}

Table oss_objects {
  id uuid [pk]
  bucket varchar [not null]
  object_key varchar [not null]
  mime_type varchar
  size_bytes bigint
  checksum varchar
  created_by uuid [ref: > users.id]
  created_at timestamp [not null]
  deleted_at timestamp

  indexes {
    (bucket, object_key) [unique]
    checksum
  }
}

Table canvas_outputs {
  id uuid [pk]
  canvas_id uuid [not null, ref: > canvases.id]
  canvas_version_id uuid [ref: > canvas_versions.id]
  oss_object_id uuid [not null, ref: > oss_objects.id]
  output_type varchar [not null]
  workflow_json jsonb
  prompt text
  status varchar [not null]
  created_by uuid [not null, ref: > users.id]
  created_at timestamp [not null]

  indexes {
    canvas_id
    (canvas_id, created_at)
  }
}

Table asset_folders {
  id uuid [pk]
  project_id uuid [not null, ref: > projects.id]
  parent_id uuid [ref: > asset_folders.id]
  name varchar [not null]
  created_by uuid [not null, ref: > users.id]
  created_at timestamp [not null]
  updated_at timestamp [not null]

  indexes {
    (project_id, parent_id, name) [unique]
  }
}

Table assets {
  id uuid [pk]
  project_id uuid [not null, ref: > projects.id]
  folder_id uuid [ref: > asset_folders.id]
  oss_object_id uuid [not null, ref: > oss_objects.id]
  name varchar [not null]
  asset_type varchar [not null]
  metadata_json jsonb
  created_by uuid [not null, ref: > users.id]
  created_at timestamp [not null]
  updated_at timestamp [not null]
  deleted_at timestamp

  indexes {
    project_id
    (project_id, folder_id, name)
  }
}

Table notifications {
  id uuid [pk]
  user_id uuid [not null, ref: > users.id]
  type varchar [not null]
  source_type varchar [not null]
  source_id uuid [not null]
  payload_json jsonb
  read_at timestamp
  created_at timestamp [not null]

  indexes {
    (user_id, read_at, created_at)
  }
}
```

## 3. 权限判断

访问 Canvas 时按以下顺序判断：

1. 用户是否为 Project Owner。
2. 用户是否为 Project Member。
3. 用户是否为 Canvas Owner。
4. 用户是否为 Canvas Member。
5. 是否存在公开分享或特殊策略。

Canvas Member 只获得指定 Canvas 的权限。

## 4. 邀请流程

```text
邀请人搜索用户
→ 创建 project_invitations 或 canvas_invitations
→ 创建 notification
→ 被邀请人接受或拒绝
→ 接受时事务性创建 member 记录
→ 更新 invitation 状态和 responded_at
→ 标记通知已处理
```

需要防止：

- 重复 pending 邀请。
- 邀请已是成员的用户。
- 无权限用户发起邀请。
- 接受已过期或已取消邀请。
- 同一邀请重复接受。

## 5. API 建议

```text
GET  /users/search?q=
POST /projects/{project_id}/invitations
POST /canvases/{canvas_id}/invitations
GET  /me/invitations
POST /invitations/{invitation_id}/accept
POST /invitations/{invitation_id}/decline
DELETE /invitations/{invitation_id}
GET  /me/notifications
POST /notifications/{notification_id}/read
```

## 6. 前端交互

Project 和 Canvas 页面分别提供邀请弹窗：

- 搜索用户名或邮箱。
- 选择角色。
- 展示已是成员、已邀请和不可邀请状态。
- 提交后显示 pending。

首页右上角信封图标显示未读邀请数量，点击后进入邀请列表。

## 7. 实现状态声明

本文件是完整设计稿，不是代码审计报告。落地前仍需要核对：

- ORM 与迁移工具。
- 当前表名和 ID 类型。
- 已有用户与权限模型。
- 软删除策略。
- 真实查询和索引执行计划。
- 前端状态管理与通知机制。