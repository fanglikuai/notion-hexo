---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CYGS5MI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCCRUFA76Zo7LAt%2FoOTHrj4UW0pkx6W85ZKb9xx7gKSLwIhAOOVIJDRKHeJdS6VqTu02P2lnXT4kkV1C5fcIrgGseyxKv8DCEcQABoMNjM3NDIzMTgzODA1IgxrGhlgiyoPSbmAraAq3AMgRtew3SCttCKgF%2FEPaOBYCxyhXl5axWqKhV4wt252%2BlckXqC3o%2BCL7D0YfpF%2BY2E7uS1o5e2FMVZf%2BhSMiiPBOWzj89TdG4BLF%2FeKj4mAsoHiaXjftBYYECk0uwEBkwVzxKOHFPMb9F0l3Qa6sHxBFQyPEeY7pc%2BMOhp1M3P2nlqdKrwmKjwghiYkYZefq14TpvmvsKUAhVaqGwprEoo7TVMbwHLtn5kDaqqasHDLmK91JGaCyqM1N7s7FdhKCn0jVho0Z51pdfInoFjBt8Za0YQvU9tMnrx%2Bmj9VYlTYwsWiYMHRtt379CyCNdHGKU%2BunyFBlEsmd7WmjfKUWRNAUz72rXtpv0LzaK1qGfQgBJ5IF1aodKC6tAB2u02ShdN7linxV3rjSCI9nBwHB9flpERviZMKd6Bb1vreNMbj6t9b37YwRSF8yQ3xX22TvRKohkbClP28gLkVlUCC1XaSLXupb1Owo%2BCZiqgWjYnDSrkh%2BaKO%2BTxFVjlD92%2F8%2F42wiGh3UuSffoFubidyb6Z3MtBkLiKUMjsUEbDtovh8IT%2BDx6lRWnnk%2BLJSnxqu3%2F2fMwmteC7AyG3plsa7EoRyKuGNaR%2F5vAhWmjR90cYvl7w5SLV7Bwcbg%2BlcXzC%2Bs%2F%2FGBjqkAfYFyQhoK14P%2FE2i%2FcHMXGK%2FyBxjEm0exoEbhLmOtnzghCrW3%2BioZB4I5z%2FP4IfstAVPJpJ6H5EnDHgRJWK2rKdQEKlSPwvYl2tI6FXJY%2BewEI4%2BM2Z9OUV26YP%2FxiADXClEQcQq5ssV8dDv%2FY5AgxR3tD6CooAs2zc4sM%2BWljGPArwckxnJPNg2ZNnMNNLJ%2BXS%2FmFoSqk3wCkyQKe%2FJsevkL7A3&X-Amz-Signature=08eb24af20fb79170ef7639505e80c57d8862c1e50553a710558f7fc47208eb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

