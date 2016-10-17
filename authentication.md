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
import java.io.UnsupportedEncodingException; import java.security.GeneralSecurityException; import java.security.KeyFactory; import java.security.NoSuchAlgorithmException; import java.security.Signature; import java.security.interfaces.RSAPublicKey; import java.security.spec.InvalidKeySpecException; import java.security.spec.X509EncodedKeySpec; import java.util.Base64; /** * Created by kael on 2016/7/7. */ public class TestJwtVerify { // sso默认开放的公钥 static String publicKey = "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDDASOjIWexLpnXiJNJF2pL6NzP\n" + "fBoF0tKEr2ttAkJ/7f3uUHhj2NIhQ01Wu9OjHfXjCvQSXMWqqc1+O9G1UwB2Xslb\n" + "WNwEZFMwmQdP5VleGbJLR3wOl3IzdggkxBJ1Q9rXUlVtslK/CsMtkwkQEg0eZDH1\n" + "VeJXqKBlEhsNckYIGQIDAQAB"; public static void main(String[] args) { // jwt token String token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6InBlbmdoaiIsInNjb3BlIjpudWxsLCJleHBpcmVzX2luIjozNjAwMCwiZXhwaXJlcyI6MTQ2NzkwMjYzOTAwMCwiZXhwIjoxNDY3OTAyNjg3MjgwfQ.P4LSoLj4cqnnNdW61HjPxWPWCvV8BdimHXp_5K0sMEAF3KRb9AFqCDAnbwWOj6OdWlIJrWt3ftBptPW7beyrKpbckRDg0YpuYLdTNKS6uJ1htpgZ5y3iRtA1r1YKl-h7GdZSqzxkXjsVH8hy03Hpg1h_TamBVQAIzonu7aclI30"; String[] parts = token.split("\\."); String content; String payload; String signature; content = parts[0] + "." + parts[1]; payload = parts[1]; signature = parts[2]; //验证token if (!verifySignature(content, signature)) { // 验证失败 } else { // 验证成功 try { byte[] decodes = Base64.getUrlDecoder().decode(payload.getBytes("UTF-8")); String info = new String(decodes); System.out.println(info); } catch (UnsupportedEncodingException e) { e.printStackTrace(); } } } // 用公钥校验token的有效性 protected static boolean verifySignature(String content, String signed) { try { byte[] signedData = Base64.getUrlDecoder().decode(signed.getBytes("UTF-8")); byte[] contentData = content.getBytes(); Signature signature = Signature.getInstance("SHA256withRSA"); signature.initVerify(decodePublicKey(publicKey)); signature.update(contentData); return signature.verify(signedData); } catch (GeneralSecurityException e) { return false; } catch (UnsupportedEncodingException e) { e.printStackTrace(); return false; } } // 生成校验用的公钥 public static RSAPublicKey decodePublicKey(String base64) { X509EncodedKeySpec spec = new X509EncodedKeySpec(Base64.getMimeDecoder().decode(base64)); try { KeyFactory f = KeyFactory.getInstance("RSA"); return (RSAPublicKey) f.generatePublic(spec); } catch (InvalidKeySpecException e) { throw new RuntimeException(e.getMessage(), e); } catch (NoSuchAlgorithmException e) { e.printStackTrace(); } return null; } } 
```

最终打印的结果如下：

```javascript{"username":"penghj","scope":null,"expires_in":36000,"expires":1467902639000,"exp":1467902687280}```

这里各个字段的含义如下：

```username:用户登录账号scope:用户授权列表，是一个字符串，以','分隔多个权限client:不一定有这个字段，只有当client是经过sso校验的情况下才会有这个字段，代表的是注册在sso的应用id,存在这个字段时表示这个token的client也是受信任的。exp:有效期```

API服务解析用户信息之后，即可以进行校验和处理了。
