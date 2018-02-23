# API 约定

本文档描述 GeekAPK API 请求方式的相关约定。

以下所涉及的 API URL 均省略由实现指定的前缀。

本文档仅对 RESTful API 设计起导向作用，并不代表最终的 API

## 请求规范
* 遵循 RESTful API
* 使用 HTTP 动词代替 URL 中出现的动词
* 请求地址一律为 `<HTTP 动词> /资源名/...`
* 返回数据类型一律为 `json`

## 资源: 用户
对应的资源为 `users`

#### 用户信息
    /users/<uid>/details

#### 用户关注的 app
    /users/<uid>/apps

#### 用户关注的人
    /users/<uid>/followings

#### 关注该用户的人
    /users/<uid>/followers

#### 用户创建的应用集
    /users/<uid>/collections

#### 用户的动态
    /users/<uid>/posts

## 其他资源以此类推

## 说明
* 如果请求的资源将会返回另一个资源的信息，那么只应该返回另一个资源中重要的信息，返回结果中需要包含另一个资源的请求方式，以便客户端获取详细信息。下面这种情况：


* 用户 1 和 2 关注了用户 0，现在客户端请求关注用户 0 的用户：
    ```
    GET /users/0/followers  
    ```

    服务端将会返回类似这样的结果:
    ```json
    {
        "followers": [
            {
                "name": "...",
                "id": 1,
                "avatar": "...",
                "refer": "/users/1/details",
            },
            {
                "name": "...",
                "id": 2,
                "avatar": "...",
                "refer": "/users/2/details",
            }
        ]
    }
    ```

* 用户 0 发布了 2 条动态，现在请求用户 0 的动态
    ```
    GET /users/0/posts
    ```
    服务端将会返回类似这样的结果:
    ```json
    {
        "posts": [
            {
                "id": 123,
                "title": "...",
                "content": "...",
                "refer": "/posts/123"
            },
            {
                "id": 456,
                "title": "...",
                "content": "...",
                "refer": "/posts/456"
            }
        ]
    }
    ```
