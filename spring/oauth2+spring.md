# oauth2+springboot

## 角色

- 资源所有者： Resource Owner 即用户具有头像、照片等资源
- 客户端：Client 客户端即第三方应用
- 授权服务器：Authorization Server 授权服务器用来验证用户提供的信息是否正确，并返回一个令牌给第三方应用
- 资源服务器： Resource Server 资源服务器提供给用户资源的服务器
- 用户代理/浏览器 User Agent

## 授权流程

1. 请求授权
2. 同意授权
3. 申请令牌
4. 发放令牌
5. 申请资源
6. 开放资源

## 授权模式

- 授权码模式 （authorization code）

- 简化模式 （implicit）

- 密码模式（resource owner password credentials）

- 客户端模式 （client credentials）


## OAuth2 Provider

OAuth2 Provider 负责公开被OAuth2保护起来的资源

OAuth2Provider的角色被分为Authorization Service(授权服务) 和Resource Service（资源服务）

在Spring Security过滤器链中有以下两个节点，这两个节点是向AuthorizationService获取验证和授权的。

- 授权节点 默认为 /oauth/authorize
- 获取Token节点 默认为  /oauth/token











  
