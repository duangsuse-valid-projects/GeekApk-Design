# 数据模型

本文档描述了 GeekApk 的数据模型。

### Conventions

- 对于所有类型的数据，其除 ID 外各项属性的值应分别保存在继承链上 **拥有对应属性** 的资源容器内，通过 ID 相关联，并通过 subtype 字段保存继承关系。

例如，对于一个 App 类型的、ID 为 `9985ea72-550f-4306-83b0-18faa09bc356` 的数据项，在数据库中应有如下表示:

Table `Topic`:

```
id = 9985ea72-550f-4306-83b0-18faa09bc356
title = ...
created_by = ...
n_likes = ...
create_time = ...
update_time = ...
subtype = App
```

Table `App`:

```
id = 9985ea72-550f-4306-83b0-18faa09bc356
package_name = ...
package_id = ...
icon_id = ...
description = ...
```

关于数据接口的约定，请参见 API.md 。

### Concepts and Relations

- Topic

Topic (主题) 是包含 **讨论** 、 **评分** 、 **收藏** 的类型。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| title | varchar(128) | 标题 |
| created_by | UUID | 创建者账户 ID |
| n_likes | u64 | 收到 Like 的数量 |
| create_time | u64 | 创建时间 (Unix timestamp, 毫秒) |
| update_time | u64 | 更新时间 (Unix timestamp, 毫秒) |
| subtype | varchar(32) | 子类型名称 |

- Comment

Implements: Topic

Comment (评论) 是用户发表的、与 Topic 相关联的文字片段。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| parent_id | UUID | 父 Topic ID |
| content | text | 内容 |

- Account

Account (账户) 是 GeekApk 认证模型的核心部分，与 Topic 等由用户创建的资源相关联，包含 **真实用户** 和机器人等 **特殊账户** 。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| name | varchar(32) | 账户名称 |
| display_name | varchar(64) | 用于显示的账户名称 |
| disabled | bool | 账户是否已被禁用 |
| icon_id | UUID | 图标静态资源 ID |
| create_time | u64 | 创建时间 (Unix timestamp, 毫秒) |
| update_time | u64 | 更新时间 (Unix timestamp, 毫秒) |
| subtype | varchar(32) | 子类型名称 |

- User

Implements: Account

User (用户) 是由真实用户拥有的账户。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| email | varchar(128) | 电子邮件地址 |
| password | varchar(128) | 经 BCrypt 加密的密码 |
| pending_auth | bool | 是否正在等待要求的额外验证 (GitHub 授权验证等) 。如果此属性为真， disabled 必须为真。|

- AccountGroup

AccountGroup 是联系 Account 与 Group 的关系类型。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| account_id | UUID | 账户 ID |
| group_id | UUID | 组 ID |
| create_time | u64 | 创建时间 (Unix timestamp, 毫秒) |

- Group

Group (用户组) 是用于划分用户权限等目的的用户分组类型。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| name | varchar(64) | 名称 |
| icon_id | UUID | 图标静态资源 ID |
| created_by | UUID | 创建者账户 ID |

- Static

Static (静态资源) 是图片、安装包等静态数据的索引类型。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| name | varchar(128) | 文件名 |
| storage_type | varchar(64) | 存储类型 (本地 / Amazon S3 / ...) |
| storage_id | UUID | 在存储提供者处对应的资源 ID
| create_time | u64 | 创建时间 (Unix timestamp, 毫秒) |

- App

Implements: Topic

App (应用) 是由开发者或管理员上传的、可被用户下载的类型。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| package_name | varchar(128) | 包名 |
| package_id | UUID | 安装包静态资源 ID |
| icon_id | UUID | 图标静态资源 ID |
| description | text | 描述 |

- Article

Implements: Topic

Article (文章) 是用户发表的文字内容，在独立的页面上展示。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| content | text | 内容 |

- Favorite

Favorite (收藏) 是用户保存的 Topic 。

属性:

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | UUID | 对象 ID |
| account_id | UUID | 账户 ID |
| topic_id | UUID | 主题 ID |
| create_time | u64 | 创建时间 (Unix timestamp, 毫秒) |
