---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624DJ54R3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T190200Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOFg3eJf%2F%2FRoSjGmKzd1vIzOoJ8qh67P0OoRNV8%2B1%2BAiAGVcafvtPOR8EmIM5P6SWczSlzwkxQz9MSgqCZdgj%2FvSqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMudBv5rTKxTidZ9GKtwDh3XgNCTseuWWSDzkEJvN9RNzO7cMBi6dDM82tsR9Y48u%2FJw8qWC7lB88pxG7f7L4xBFcCx%2F5Jo4xDC8Z6eiVjKZJv3Yclb%2BI1%2BPhFUUB5IKYzIjODUl7wt9wnp2nM%2FJbIV9ad8AAbxtHD5fu2kKMMRgyE4JMdVm59P4ubJGOC98ntty8JWN7MmCOmdfBfS3CruP%2Ba%2FW3%2BzBNDaC8ysRkEgQ%2FatT5S3QRlqrU2Se5aEhacX9zN3Ixxj2RGboKqeaVSgW9D1E655QPTa6MgyCYCgW7OaudPULadfDeA81Zt1dkU7FHQ9l%2FlJeMH9sA4DrujvO3xmfrN%2Fpeu2Fu57hEGiQ%2BSk5lZdhoxhMterdx8bx%2FtOsr42BN06mtN3L%2BYvvpp5e25x0heglU1nj5FZ3q4XSvvcNHX7uajGsNXvCqOJD1s9zMTMQrrGDePIwfdJdqzHU0zRt9Ywdq8Uugcri27SvoHMUESYs1Mt5AdMRnO163ysganE4ka2uL1OXooUYQ%2FPWIIFUBNIbd7%2BS0rqWiZZmrHkfvaY5XkI0hfa1OEHr9tUXvtj63FxCQxYolG8KkiZtLv9nCNK2B4aWsDL%2Ftg2PQECa4r7WiZOni3GnrDRtAIe%2BHBr%2BbMw2giMgwhPfExwY6pgFWUhd7sEuIpAbPLqrkM4%2BXg7iv60cCq0tBJaGXa0GTEewockQbSC1ux8WDH8CkYhgJI39D5dldJS7lKmwNv%2BRWCJmrefS8gpCG24auYXQFwffSoHo9YvsGTgxBVeGaB7idaGyTMcunnx1Vi7gec4zEjgGTtSLlXJn2wZiFLzhcUBNOMWy6UhONNfB6r7maRDqo9VsO59wN2XiHkJNC8X%2FuN2nc4LmJ&X-Amz-Signature=74113c37498689f9c52082c5d4d1af8efe013fdb145c60e74e45e46d4ef5d4c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:20:00'
index_img: /images/ba77b23d1f7fbe3158ca80a71d20f446.jpg
banner_img: /images/ba77b23d1f7fbe3158ca80a71d20f446.jpg
---

# 加密算法


HTTPS解决数据传输安全的方案就是使用加密算法，具体来说就是混合加密算法，也就是对称加密和非对称加密的混合使用。


## 对称加密


顾名思义就是加密和解密都使用同一个密钥，常见的对称加密算法有DES，AES等


优点：

- 算法公开，计算量小，加密速度快，加密效率高，使用加密比较大的数据

缺点：

- 双方使用同样的密钥，需要传输密钥，可能会被截获，不安全
- 密钥每次都要不同，需要管理大量的密钥

## 非对称加密


使用公钥和私钥，常见的算法有RSA算法

- 优点：算法公开，加密和解密使用不同的钥匙，私钥不需要通过网络进行传输，安全性很高。
- 缺点：计算量比较大，加密和解密速度相比对称加密慢很多。

# 原理解析


官方图片：


![imagesd44b6927dda25ed87175d2417755aa00.png](/images/3dc3885631aadf23c5728c49bb5df3c4.png)


我的图片


![image.png](/images/7dac926f4b3925358a887a46c786b703.png)


采用 HTTPS 协议的服务器必须要有一套数字 CA (Certification Authority)证书，证书是需要申请的，并由专门的数字证书认证机构(CA)通过非常严格的审核之后颁发的电子证书 (当然了是要钱的，安全级别越高价格越贵)。颁发证书的同时会产生一个私钥和公钥。私钥由服务端自己保存，不可泄漏。公钥则是附带在证书的信息中，可以公开的。证书本身也附带一个证书电子签名，这个签名用来验证证书的完整性和真实性，可以防止证书被篡改。


服务器响应客户端请求，将证书传递给客户端，证书包含公钥和大量其他信息，比如证书颁发机构信息，公司信息和证书有效期等。Chrome 浏览器点击地址栏的锁标志再点击证书就可以看到证书详细信息。

