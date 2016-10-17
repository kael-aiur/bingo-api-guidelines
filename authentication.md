# 用户身份校验

对于大多数api来说，用户身份校验是必不可少的，在网关中，用户身份校验已经通过了，网关转发到api的请求中包含了请求头:

```
Authorization Bearer ${JWT_token}
```

这个请求头表明网关已经校验过用户身份了，我们可以从`JWT_token`中解析出用户的相关信息。

这里我们先对JWT token做一个简单的解释：

* jwt token分为3段，以'.'分隔；
* 第一段是头部(Header)，主要描述jwt的基本信息如：加密算法，token类型等，是一个经过base64编码的json字符串；
* 第二段是载荷(Payload)，主要是存放真正的用户信息，也是一个经过base64编码的json字符串；
* 第三段是签名(signature)，主要是把第一段和第二段用'.'拼接之后用私钥加密后的签名信息。

了解jwt token的组成之后，接下里我们看看如何解析jwt token。

* 要解析jwt token，首先需要验证jwt_token的有效性，这个可以通过sso公布出来的公钥信息进行校验，校验的是字符串是`${Header}.${Payload}`，校验通过的话说明这个jwt token是没有被篡改过的，是受信任的；
* 解析用户信息，取出Payload这段，并经过Base64解码即可得到JSON字符串，这个JSON中包含了用户相关的信息。

这里我们以JAVA为例，示例一个JWT token校验解析的过程：

```java

```

最终打印的结果如下：

```javascript{"username":"penghj","scope":null,"expires_in":36000,"expires":1467902639000,"exp":1467902687280}```

这里各个字段的含义如下：

```username:用户登录账号scope:用户授权列表，是一个字符串，以','分隔多个权限client:不一定有这个字段，只有当client是经过sso校验的情况下才会有这个字段，代表的是注册在sso的应用id,存在这个字段时表示这个token的client也是受信任的。exp:有效期```

API服务解析用户信息之后，即可以进行校验和处理了。
