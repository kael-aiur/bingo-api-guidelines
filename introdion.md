# api网关总体介绍

品高api网关是一套完整的大型企业应用，包含了请求代理，用户身份校验，负载均衡，分布式调用跟踪日志和日志分析，客户端sdk生成和接口文档生成等功能。

## 网关的部署架构

整个网关的运行架构如下：

![网关运行架构](/assets/网关整体架构.png)

处理一次请求的过程如下：

* 所有请求都通过网关路由接收；
* 路由接收请求后，会先通过sso进行用户身份验证，并获取sso颁发的用户身份标识令牌(token)；
* 路由获取token之后，请请求转发到真正的api；
* api通过token确认用户身份，并处理请求，返回处理结果；
* 网关将返回处理结果返回给客户端；
* 整个调用过程的日志都通过ELK进行收集和监控；

在上述整个过程中，api需要特别关注的就是网关请求转发过来时的token，这个token包含了用户身份和权限列表，用于api本身的用户身份校验和权限校验。

## OpenId Connect

网关和sso之间的用户身份校验是通过[OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)协议实现的，并且网关通过身份校验之后，会将用户信息转成[JWT token](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)并在转发给api的请求中添加如下请求头：

```
Authorization Bearer ${JWT_token}
```

这个请求头是oauth2身份校验的标准请求头。