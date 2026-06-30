# ComfyUI_Bench：数据库设计整理

## 1. 数据库设计目标

这个项目的数据库需要支持：

- 用户系统。
- Project 管理。
- Canvas 管理。
- Project 成员权限。
- Canvas 成员权限。
- 邀请记录。
- 版本管理。
- 生成输出。
- OSS 对象存储。
- 项目素材库。

核心层级：

```text
User
  ↓
Project
  ↓
Canvas
  ↓
CanvasVersion
  ↓
CanvasDetail / CanvasOutput
```

资产层级：

```text
Project
  ↓
Folder
  ↓
Asset
  ↓
OssObject
```

## 2. 为什么需要 Project 和 Canvas 两层权限

之前讨论中明确过一个关键点：

```text
Project 的权限可以直接复用给所有 Canvas，
但是 Canvas 的权限不代表复用给 Project。
```

也就是说：

- Project 是上层容器。
- Canvas 是 Project 下的具体工作区。
- Project 成员默认可以访问 Project 下的 Canvas。
- Canvas 可以额外邀请协作者。
- 但一个人被邀请到某个 Canvas，不应该自动获得整个 Project 的权限。

这个设计非常重要。

## 3. 建议核心表

### users

保存用户基本信息。

```text
users
- id
- username
- email
- password_hash / auth_provider
- avatar_url
- created_at
- updated_at
```

### projects

保存项目。

```text
projects
- id
- owner_id
- name
- description
- visibility
- created_at
- updated_at
```

### project_members

保存 Project 成员关系。

```text
project_members
- id
- project_id
- user_id
- role
- joined_at
- invited_by
```

### project_invitations

保存 Project 邀请记录。

```text
project_invitations
- id
- project_id
- inviter_id
- invitee_id
- role
- status
- created_at
- responded_at
```

这里保留邀请记录是必要的，因为需要支持：

- 首页信封通知。
- pending 状态。
- accept / decline。
- 防止重复邀请。
- 审计邀请来源。

### canvas

保存 Canvas。

```text
canvas
- id
- project_id
- owner_id
- name
- description
- created_at
- updated_at
```

### canvas_members

保存 Canvas 独立成员关系。

```text
canvas_members
- id
- canvas_id
- user_id
- role
- joined_at
- invited_by
```

### canvas_invitations

保存 Canvas 邀请记录。

```text
canvas_invitations
- id
- canvas_id
- inviter_id
- invitee_id
- role
- status
- created_at
- responded_at
```

### canvas_versions

保存 Canvas 版本。

```text
canvas_versions
- id
- canvas_id
- version_number
- created_by
- created_at
- note
```

### canvas_details

保存版本对应的详细状态。

```text
canvas_details
- id
- canvas_version_id
- data_json
- created_at
```

### canvas_outputs

保存生成结果。

```text
canvas_outputs
- id
- canvas_id
- canvas_version_id
- oss_object_id
- output_type
- prompt
- workflow_json
- created_by
- created_at
```

### oss_objects

保存 OSS 对象元数据。

```text
oss_objects
- id
- bucket
- object_key
- url
- mime_type
- size
- checksum
- created_at
```

### asset_folders

保存项目素材库文件夹。

```text
asset_folders
- id
- project_id
- parent_id
- name
- created_by
- created_at
```

### assets

保存素材。

```text
assets
- id
- project_id
- folder_id
- oss_object_id
- name
- asset_type
- created_by
- created_at
```

## 4. 权限判断原则

判断用户是否能访问 Canvas 时：

```text
1. 是否是 Project owner
2. 是否是 Project member
3. 是否是 Canvas owner
4. 是否是 Canvas member
5. 是否存在公开权限或特殊分享规则
```

注意：Canvas member 不能自动反向成为 Project member。

## 5. 邀请系统状态

邀请状态建议：

```text
pending
accepted
declined
expired
cancelled
```

## 6. 后续要补充

- 完整 V3 schema 代码。
- ER 图。
- 索引设计。
- 唯一约束。
- 删除策略。
- 权限检查 SQL。

