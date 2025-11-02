---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXKS2KYI%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBetCnxvmxzMm36C69fjX9557MBuSUCBcZZUCLrb4OiuAiAkbioLa%2BqoiX%2F0dKHeLTzscJ35fvGZmUzQsDRDVtjlPSr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMl7JBZsmVfBfrne8vKtwDNz3ls9Ouqr60%2B%2BMc8scTHPgLlaYqMDG0d%2B3AJkL9ZUOi0blUsumgNjL%2B%2B4mtuF1FEFuGilA03zThiyoufvjUiOJQvZSFhvdk%2FteQo6x6ip01pMEGMR%2BhMv9vsAWxYh5a8c1%2FkwliPnIDEHMuujRvG2QPFAXTNe69NtjwLsDGLf9AUmUVRqed2W4migWfxsEMwFVKLgnE8l7cXCG3sJV1QYssmw%2FEFuwY0EAsbzBpDU6pkwModyR9tf9dLlcCdQj%2FN30ur%2BIGbGXCIEwZsCjc4w24St1YA960HFJOz2zMW9dIxNf%2BR7g5S8NCzudSTopDKrnqKXWj98XMw%2BgKYeCaEKmsAj90oz%2FlpRcTlVaDNDKMXtMBfaLEq8eagvd%2BsaS%2FQx1a1iAfP24lCRnk9w%2F51aE3a9NiZzzWznUdA6IVXaN3ztEUEpddcmDnWrwltKRfqJ%2Bt1JdlIWsfOQfbn9UN6TOaaCpqhPd94UY6yErhEQPlWnMSWFzTn69KZU4C8%2FPz4Aj6GO2zaVMXNzE7JWTiw0whSVmlOFR4NBfyl4qGSGoyRbXhN%2Bb8smmNAnZPb9SISz0JHfKrPjiTTEZODJo9dgMQ48UPy1scPtG1TrsLv7i%2F8%2Fu8I3EuOQv9qfcw772eyAY6pgHj%2BN3h2Xd1pYL2AkSV0mY58MIy8zWMF0M0F1jp3jpFSYq9mbfO9ycAXlc6H%2FWE1QRHK4NpRb%2BE%2FjGz1bihc%2FvRCZ8OifFaxaV61FV2IVvBd%2FXfY%2FYBSUelnARcDEVPalQnl3Um1UoZuz0rPaD6we7i7kWKCg9zH7GQG9B6LrGlFU56y7Rt52GX%2BxZ8SK0UMC8%2B0bY3DsySNwZE0eQTLAcsHQsnyQQf&X-Amz-Signature=8e96a12e1a112dcd5bf87f12282646a7bc95ca98da2d9b3a0550a4b059c2e1c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

