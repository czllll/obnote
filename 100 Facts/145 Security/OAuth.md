---
tags:
- protocol-standard
---
|                    |       |       |

## OAuth 2.0
### 角色
* **资源所有者 (Resource Owner)**：通常指用户，也就是你，拥有数据和资源的最终控制权。
- **客户端 (Client)**：希望访问用户资源的第三方应用程序，例如某个需要读取你 Google 相册的图片打印网站。
- **授权服务器 (Authorization Server)**：负责验证用户身份，并在获得用户授权后，向客户端颁发“访问令牌”的服务器。例如 Google 或微信的认证服务器。
- **资源服务器 (Resource Server)**：托管受保护资源的服务器，它会验证客户端出示的“访问令牌”，如果有效，则向客户端提供其所需的资源。例如 Google Photos 的 API 服务器。
- 在很多情况下，授权服务器和资源服务器可以是同一个服务提供商（比如 Google），甚至是同一个服务器。
### 流程

1. 用户发起授权请求 
2. 客户端重定向用户到授权服务器
	1.  `photo-print.com` 的服务器会将你的浏览器重定向到 Google 的授权服务器，并在 URL 中附带一些信息，例如：
		- `client_id`：表明 `photo-print.com` 是谁。
		- `redirect_uri`：告诉 Google 授权成功后应该跳转回哪个地址。
		- `scope`：`photo-print.com` 希望获得的权限，例如“只读访问你的相册”。
		- `response_type=code`：表明希望使用“授权码”模式。
3. 用户向授权服务器进行授权 你的浏览器会跳转到 Google 的登录和授权页面。
4. 授权服务器返回授权码 (Authorization Code) 
	1. Google 的授权服务器会把你的浏览器重定向回 `photo-print.com` 在第2步中指定的 `redirect_uri`，并在 URL 后面附上一个临时的**授权码**，例如 `https://photo-print.com/callback?code=ABC123XYZ`。
5. 客户端使用授权码交换访问令牌 
	1. `photo-print.com` 的**后端服务器**收到了这个授权码。它会立即在后台向 Google 的授权服务器发起一个请求，这个请求包含了：
		- 刚刚收到的**授权码**。
		- 它自己的 `client_id` 和 `client_secret`（用来证明自己的身份）。
6. 授权服务器颁发访问令牌 (Access Token) 
	1. Google 的授权服务器验证收到的授权码和客户端信息无误后，会向 `photo-print.com` 的后端服务器颁发**访问令牌 (Access Token)** 和一个可选的**刷新令牌 (Refresh Token)**。
7. 客户端使用访问令牌访问资源 
	1. `photo-print.com` 现在拥有了访问令牌。当它需要获取你的照片列表时，就可以向 Google Photos 的资源服务器（API）发起请求，并在请求的 `Authorization` 头中附上这个访问令牌。
8. 资源服务器返回受保护资源 
	1. Google Photos 的资源服务器收到请求后，会验证访问令牌的有效性（是否过期、权限是否足够等）。验证通过后，就会将你的照片数据返回给 `photo-print.com`。

### 令牌 (Tokens) 的类型

- **访问令牌 (Access Token)**：
    - 是客户端访问受保护资源的凭证。
    - 通常是无意义的字符串，具有时效性（例如，1小时后过期），且权限范围受限（例如，只能读取照片，不能删除）。
    - 客户端的每次 API 请求都需要携带它。
- **刷新令牌 (Refresh Token)**：
    - 是用来获取新的访问令牌的凭证。
    - 它的有效期通常很长（比如几天、几个月甚至永久）。
    - 当访问令牌过期后，客户端可以使用刷新令牌向授权服务器申请一个新的访问令牌，而不需要用户再次进行登录和授权操作，提升了用户体验。

## OpenID Connect (OIDC)
* 虽然 OAuth 的核心协议是关于**授权 (Authorization)**（即“你能做什么”），但它通过一种巧妙的方式被扩展用于**认证 (Authentication)**（即“你是谁”）。这个基于 OAuth 的认证流程，后来被标准化为一个新的协议，叫做 **OpenID Connect (OIDC)**。
### 流程
假设你正在登录一个名为 `awesome-blog.com` 的博客网站：
1. **发起登录请求**：你在 `awesome-blog.com` 上点击“使用 Google 登录”。
2. **重定向与授权**：网站将你重定向到 Google。你输入 Google 的用户名密码，并同意 `awesome-blog.com` 查看你的基本个人信息。这里请求的权限 `scope` 通常是 `openid`, `profile`, `email` 等，而不是 `photos.read` 这种资源权限。
3. **获取授权码**：Google 将你重定向回 `awesome-blog.com`，并附上一个 `authorization_code`。
4. **交换访问令牌**：`awesome-blog.com` 的后端服务器用这个 `code` 和自己的凭证换取了 `access_token`。
    
**--- 关键区别从这里开始 ---**

现在 `awesome-blog.com` 拿到了 `access_token`，但它的目的不是去访问你的 Google Photos 或 Gmail。它的目标是：**确认你的身份**。

5. **调用用户信息接口 (User Info Endpoint)**：
    - `awesome-blog.com` 的服务器会立即使用这个 `access_token` 去请求 Google 的一个特殊**资源服务器**接口，这个接口通常被称为 “User Info Endpoint”（例如 `https://www.googleapis.com/oauth2/v3/userinfo`）。
    - 这个接口的作用就是返回授权用户的基本公开信息。
6. **获取用户信息并完成登录**：
    - Google 的 User Info 接口验证 `access_token` 有效后，会返回一个包含你信息的 JSON 对象，例如：
        ```json
        {
          "sub": "1098......", // 你在Google的唯一ID
          "name": "张三",
          "given_name": "三",
          "family_name": "张",
          "picture": "https://lh3.googleusercontent.com/a/....",
          "email": "zhangsan@gmail.com",
          "email_verified": true,
          "locale": "zh-CN"
        }
        ```
        
    - `awesome-blog.com` 的后端拿到这些信息后，执行以下逻辑：
        - **检查数据库**：通过返回的 `sub` (唯一ID) 或 `email` 查找自己的用户数据库。
        - **如果是老用户**：发现数据库里已经有这个 Google 用户 ID，就直接为你创建网站自己的登录会话（Session 或 Cookie），登录成功！
        - **如果是新用户**：发现数据库里没有这个用户，就会用获取到的信息（如姓名、邮箱）在自己网站的用户表里创建一个新账户，并将这个新账户与你的 Google 用户 ID 关联起来。然后，为你创建登录会话，注册并登录一步完成！