---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UW3KPG24%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T020057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICMdzFICoZIYFCnD8TvTQ4Zn0L7OxafwLze0b8WT0keUAiEAublpNZXwCea3DNS8pQMARoGQHYUZ9IgP7hvWw%2F%2FrHaAq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDLrE2N1fG%2BODkvuSxSrcAyX1%2BLdDFa3SoWGU7iQ19XjgIGYyCdUh0D598vHrjMwNmfBGSybkNUBeEVwLd8UMR0kiDG26KK0Lq82oYraDaJ56qqJUu7J%2FA8En5qxcIRyNPkFd52qh9cHQ6c7n1DsZ4wDM3OojEqViLoCo6qV%2BFoCYd3Fej2HX4mnx96EfFGv%2BArGBcn9jXYnNcY4Wkr3DzGpQC1%2FFgu0Q8vDCJxk%2BynOPbIDS%2BeVr4pWANKrEI70KKZIhYmpePDhJu4XjbEkhLLj%2B5lJCd0MQoVQbH2Od%2FF%2F9NKjamlA%2FWbdTzhUCN7Ufuh04r5SqcvXOvjLTsX6m79CkgtJttt9TYT9u28qKOTTKQ%2FgY5%2BlSnBIvqfiRaix1xz%2FZUy%2FFJ2jjiEek%2BaySvlGGlz00nq77iJcxOhppuPki7Jd9M%2BFSlTL69aqr0VHRihEcJ68jvl3yMGzjaJXoJKlJsjGkM2lfgfNiOQkM0X1oUsPff3fKceV0huWRTGC9k7xiG7TePSd81O472PpdlxMNIeD5fFfH2pTAo3fZAZtxctl04v6C5IGQydw0wZykaipoZfQjcrJ%2FHqC8wFdjo6XYdXbwgV0RNVGaXeecLQIFkkHk%2FBwjGwv%2F78fpZDL2t%2FXCiMF0DpwGPiuSMKXKjskGOqUBrKz36RsVIKKIIr0LwBlSSIX0a8GEy2NvixDEJSF%2FdnfzUVVrIuFb00mugUIt4YcCTk9xjVN8RzlNrrhVKOgJaOdj%2BwpzLC0I552OJd8Hb92UwlTKz2LhnZgKYjk2rR4vYZ4T0xoMoW1J2YhIH%2B8GLKl9xfuDP2M5o%2B3%2B%2Fcr7kZHp2h1qCzNLgi1l0wZy%2Bcv8p64WJS9RSKLxLR5xGL3Nqo7HRrNO&X-Amz-Signature=51843a004240d110698acc2c3baa09674c1e123d6b82ca445e2fe736e3b2504d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

