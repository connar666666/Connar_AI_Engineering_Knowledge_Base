# Email MCP：协议、线程、认证与权限设计

## 1. SMTP、IMAP、POP3 的职责

### SMTP

SMTP 负责发送邮件：

```text
邮件客户端 / MCP
  ↓ SMTP
发件邮箱服务器
  ↓ SMTP
收件邮箱服务器
```

SMTP 不负责长期同步收件箱状态。

### IMAP

IMAP 负责在服务器上读取和管理邮件，支持多设备同步：

- 列出文件夹。
- 搜索邮件。
- 获取邮件头、正文和附件。
- 标记已读、未读、星标。
- 移动或删除邮件。
- 按 UID 做增量同步。

企业 Email MCP 通常应优先使用 IMAP，而不是 POP3。

### POP3

POP3 更接近“把邮件下载到本地”，对服务器端文件夹、状态同步和多设备协作支持较弱，不适合作为完整 Email Agent 的主要协议。

## 2. IMAP UID 与增量同步

邮件的序号可能在邮箱状态变化后改变，因此不应把 sequence number 当成稳定主键。

推荐保存：

```text
account_id
mailbox_name
uid_validity
uid
message_id
internal_date
```

增量同步基本流程：

```text
读取本地 sync cursor
  ↓
检查服务器 UIDVALIDITY
  ↓
按 UID 范围拉取新增邮件
  ↓
解析并持久化元数据
  ↓
更新 cursor
```

如果 `UIDVALIDITY` 改变，原 UID 空间可能失效，需要触发该文件夹重新同步或重新建立映射。

## 3. 需要确认的 IMAP 扩展

- `UIDPLUS`：更可靠地处理 COPY、APPEND 等操作后的 UID。
- `IDLE`：服务器有新邮件时进行近实时通知。
- `CONDSTORE`：通过修改序列减少全量状态同步。
- `QRESYNC`：更高效地恢复断线后的变更状态。
- `SORT`：由服务器排序。
- `THREAD`：由服务器提供线程聚合结果。

不能假设企业邮箱一定支持这些扩展，必须通过 CAPABILITY 和服务器团队确认。

## 4. 邮件线程模型

IMAP 标准本身不保证存在统一的 `thread_id`。跨邮箱系统最通用的聚合依据是 RFC 邮件头：

```text
Message-ID
In-Reply-To
References
Subject
From / To / Cc
Date
```

### 基础规则

1. `Message-ID` 标识单封邮件。
2. `In-Reply-To` 指向直接父消息。
3. `References` 保存祖先消息链。
4. 主题只作为辅助信号，不能单独作为线程依据。

### 推荐内部模型

```text
email_messages
- id
- account_id
- mailbox
- uid
- message_id
- in_reply_to
- references_json
- normalized_subject
- sent_at
- sender
- recipients_json
- body_ref

email_threads
- id
- account_id
- root_message_id
- normalized_subject
- last_message_at
- message_count

email_thread_members
- thread_id
- email_message_id
- parent_message_id
- sort_order
```

### 聚合算法

```text
优先使用 References / In-Reply-To 建图
  ↓
找到根 Message-ID
  ↓
同一根节点归入同一线程
  ↓
对缺失头信息的邮件使用主题、参与人和时间窗口兜底
  ↓
对低置信度聚合保留 confidence 和原因
```

转发邮件通常不等价于回复线程，除非系统能够从正文或原始头中可靠恢复引用关系。

## 5. 线程聚合的两层设计

### 协议线程

严格依据邮件头建立回复链，适合“查看这封邮件的上下文”。

### 业务线程

通过主题、实体、项目编号、合同号、客户、附件和语义相似度，把多个技术线程聚合成同一业务事件。

```text
协议线程：邮件系统事实
业务线程：Agent 推断结果
```

两者必须区分，避免把模型推断误认为邮箱服务器原生关系。

## 6. 第三方客户端授权

已验证的现状是：用户可以创建第三方客户端专用密码，并通过 IMAP/SMTP 访问邮箱。

需要关注：

- 授权码是否长期有效。
- 是否可以撤销和重新生成。
- 是否与普通登录密码隔离。
- 是否支持管理员统一开启。
- 是否支持应用级 OAuth、服务账号或委托访问。
- 是否存在单用户连接数和调用频率限制。

授权码不能明文保存。推荐使用密钥管理系统加密，并通过短生命周期解密流程注入运行时。

## 7. SSO 与邮箱账号映射

OA Agent 已经有企业微信或 OA 登录身份，但这不代表天然获得邮箱访问凭据。

推荐建立映射：

```text
OA user_id
  ↔ employee_id
  ↔ corporate_email
  ↔ email credential reference
```

Credential reference 指向密钥系统中的凭据，而不是在业务数据库中直接保存明文密码。

## 8. 权限责任划分

### OA Agent 平台

- 识别当前用户。
- 判断是否允许调用 Email MCP。
- 绑定用户邮箱。
- 发送前确认。
- 审计和策略控制。
- 限制外发、群发、附件和敏感内容。

### Email MCP

- 检查调用上下文是否包含合法 account scope。
- 禁止跨账号访问。
- 标准化 IMAP/SMTP 错误。
- 对危险工具增加确认标记和幂等键。
- 不信任模型直接提供的任意邮箱凭据或账号 ID。

### 邮箱服务器

- 用户邮箱权限。
- 服务端协议能力。
- 连接与频率限制。
- 群组、通讯录和可能存在的发送策略。

## 9. 抄送与越级沟通

从邮件协议角度，`Cc` 是发件动作中的一个收件人字段；普通用户能否抄送某人，通常取决于邮箱服务器策略和企业制度，而不是 MCP 自行定义。

Email MCP 应支持：

- To、Cc、Bcc 的结构化参数。
- 外部域名检测。
- 高层级人员或敏感群组的策略校验。
- 大范围收件人的二次确认。
- 发送结果审计。

## 10. 关键结论

1. IMAP 是读取与同步的主协议，SMTP 是发送协议。
2. 不应假设存在原生 thread id。
3. 线程重建应优先使用标准邮件头，而不是只看主题。
4. SSO 身份、邮箱账号和邮箱凭据是三个不同概念。
5. MCP 不应成为企业权限体系的唯一边界。