Q ：http 与 https 区别

A : http 是 HTTP 协议运行在 TCP 之上。所有传输的内容都是明文，客户端和服务器端都无法验证对方的身份。

https 是 HTTP 运行在 SSL/TLS 之上，SSL/TLS 运行在 TCP 之上。所有传输的内容都经过加密，加密采用对称加密，但对称加密的密钥用服务器方的证书进行了非对称加密。此外客户端可以验证服务器端的身份，如果配置了客户端验证，服务器方也可以验证客户端的身份。

一个简单的结论：简单的来说 HTTP 和 HTTPS 的区别是有没使用 SSL（安全套接字层）或者其后继者 TLS（传输层安全）

https 需要证书和提供安全连接。 
http 信息是明文传送，https 是嵌套了 SSL 加密的 http 连接，其内容会由 SSL 先加密，然后再传送。


**Https的作用：**

内容加密 —— 建立一个信息安全通道，来保证数据传输的安全；
身份认证 —— 确认网站的真实性
数据完整性 —— 防止内容被第三方冒充或者篡改

**Https的劣势：**

对数据进行加解密决定了它比 http 慢：
    需要进行非对称的加解密

Q : SSL 四次握手

![](http://image.beekka.com/blog/201402/bg2014020502.png)

1. 客户端发出请求（ClientHello）

携带信息：

（1） 支持的协议版本，比如TLS 1.0版。

（2） 一个客户端生成的随机数，稍后用于生成"对话密钥"。

（3） 支持的加密方法，比如RSA公钥加密。

（4） 支持的压缩方法。

2. 服务器回应（SeverHello）

携带信息：

（1） 确认使用的加密通信协议版本，比如 TLS 1.0 版本。如果浏览器与服务器支持的版本不一致，服务器关闭加密通信。

（2） 一个服务器生成的随机数，稍后用于生成"对话密钥"。

（3） 确认使用的加密方法，比如RSA公钥加密。

（4） 服务器证书。

3. 客户端回应

携带信息：

（1） 一个随机数。该随机数用服务器公钥加密，防止被窃听。

（2） 编码改变通知，表示随后的信息都将用双方商定的加密方法和密钥发送。

（3） 客户端握手结束通知，表示客户端的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供服务器校验。

4. 服务器的最后回应

携带信息：
（1）编码改变通知，表示随后的信息都将用双方商定的加密方法和密钥发送。

（2）服务器握手结束通知，表示服务器的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供客户端校验。

简单来讲原理即为：

    当你的浏览器向服务器请求一个安全的网页(通常是 https://)，服务器就把它的证书和公匙发回来

    浏览器检查证书是不是由可以信赖的机构颁发的，确认证书有效和此证书是此网站的。

    之后使用公钥加密了一个随机对称密钥，包括加密的 URL 一起发送到服务器

    服务器用自己的私匙解密了你发送的钥匙。然后用这把对称加密的钥匙给你请求的URL链接解密。

    服务器用你发的对称钥匙给你请求的网页加密。你也有相同的钥匙就可以解密发回来的网页了


Q: 优化手段

A: 

1. False Start
False Start 有抢跑的意思，意味着不按规则行事。TLS False Start 是指客户端在发送 Change Cipher Spec Finished 同时发送应用数据（如 HTTP 请求），服务端在 TLS 握手完成时直接返回应用数据（如 HTTP 响应）。这样，应用数据的发送实际上并未等到握手全部完成，故谓之抢跑。

2. Session Resumption
另外一个提高 TLS 握手效率的机制是会话复用。会话复用的原理很简单，将第一次握手辛辛苦苦算出来的对称密钥存起来，后续请求中直接使用。这样可以节省证书传送等开销，也可以将 TLS 握手所需 RTT 减少到一个.

3. 减小证书大小等..