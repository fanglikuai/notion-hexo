---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVRRE4X%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIALNNbwvCotw7sUyu1va1Xk23rCFMlgHx2Vj3M1o5tDvAiAhfJyQdR8ErzVHOm%2F3GlWbrfbW0dV49%2BcTJNlvrx%2B9jCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMfl7%2FRED75odPinPcKtwDcJFwXIwj7cyDvHqf9GgYec0CsSgfVjDVdq2FeEnJ7vYPvZnZL8xXipEx2c7XDExEdL1FYYrxjKH7z7l12OaNLZRKFm7SM3zxr1%2FUhB%2Bt9AH4%2FNDWuAezJOaHBGxE4JgkCRwgsvrzTAEaCXTaB%2F0qpL8cepurXJqiY4o7zqDVqY6wb8WqI%2FkN8pKLB7J0zpgJQ22X5Lx%2Bd%2BNFhkIOS4nN6R7vhpKoYqI15oWZotaundnSMg2BrIHUDEKsbrGalDS7ePlPHfOlxsiYq%2ByQzWLbaPQQ%2FWe6T57O3hnsq1NZLLV1zr3q%2BG9ugtvtI2kmD4Nm3iJDVSYWhjPx%2Bs6O%2BTN8x%2F3sRXA6%2FxeIpHG%2BOp8AFGUEA4vI4RrgXygyXxib0SHx7YB%2FkSAYYwj6lilKPCv2nitISUsVHEaQViL4%2Flt%2BgnYXOXCkIUktAZAoMsu9u088jD4x45ciHmA9t%2FGug14jGpIXVCpEtPA54IGr5QWjLdIVeg5b6xkMmsLY%2BskxxtNKtG%2FmTi%2BCr6KujXX55y2ljgEU6cinvz6Rxl%2B1Su7JIiHaBq%2Bge06jKUdB7Jz1xjfgEtvSCxpK3QwNQ6ffx1r2nEW3BjqXy%2FRMFRIWb5WUctf%2BLyaCX3JngAQJBOgwroDMxgY6pgEhSArYwO1f8fsU5qgi46or5093vQ3OI2i5pt2bEQlwUdqeJyahSMSxx98AX0nE86dorge4NOem2m5l%2F03ne82Mkt93IYLQsmg6JabHtAcr7En8jNt9M4NLRwqUjCpeXszz35PnBr8b71uB0OqJO03WizHNBUPqjzLtlvBGC8M4FLLcIlNqKuMHe0TRqm5a0fT%2ByPWMkWLUjBW6z5Xh8yPoNv6AAgJe&X-Amz-Signature=a27c58c4cc6b8873ef83ff6fabe8f8e30a7a4e948cf434b4f9949bbb1957d921&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

