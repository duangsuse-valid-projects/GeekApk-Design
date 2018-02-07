# API 约定

本文档描述 GeekAPK API 请求方式的相关约定。

以下所涉及的 API URL 均省略由实现指定的前缀。

### Format

API 请求第一级路径应与 Model.md 中定义的模型名称相对应。

当所请求的资源类型继承了其他类型时，除特殊情况外，处理该类请求的 API 应位于 **请求的字段所属类型中，位于继承链最下端 (最接近具体类型) 的类型所对应的路径** 下。

例如，对于一个 App 类型的、ID 为 `9985ea72-550f-4306-83b0-18faa09bc356` 的数据项， API 应有如下实现：

- 新增:

路径: /App/new (访问了只属于 App 类型的字段)

参数: `title`, `package_name`, `package_id`, `icon_id`, `description`

- 移除:

路径: /Topic/remove (所有访问的字段均属于继承链最上端的 Topic 类型)

参数: `id`

(后端处理时，应通过 subtype 找到具体类型完成彻底的删除)

- 修改:

路径: /App/update (有可能访问了只属于 App 类型的字段)

参数: `title`, `package_name`, `package_id`, `icon_id`, `description` (均可为空)

- 读取:

路径: /App/get (访问了只属于 App 类型的字段，包含于接口返回数据中)

- 特殊情况:

例如，获取应用推荐列表时，即使接口只返回了 `title` 和 `id` 字段，该接口也显然应位于 `/App` 下。
