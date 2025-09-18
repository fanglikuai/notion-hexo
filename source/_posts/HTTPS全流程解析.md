---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B3IXJSZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJFMEMCH1lvxEaUkD3qri6zbfywMFH7WbabStkMC%2FhY9is%2FW4kCIGW5ZbFl8Qv17%2BYHrw8GhmQQIRuMoPldfEDAHTRIhD80KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwuKTJ7xiGJYC8HhQgq3APG%2Bv9YYwQKZlgd2ABMUG%2BrxSjTJs8vvnkdipHU3DqkmTSvBQXv1YPdaKaq2uxNkF4rfx7D9jUM%2Fdz35NUUJwVwOh%2FvfN%2Bv3C%2FUWgNOEZwF8LElVtdGjZygWxJG%2BTGF3dkFyRyIT5pY3uUyy4glnsZafu3YYULvns0OVESnEicG4%2FQpZx5X8dWEVzbK5SD1tSXQ2LrrG43R0mf5vtz6bBHA1bqxydHDQhJ42AMZlAGQx1A1gTM8qr0G2jnTLIm5pPULDOBeiXV9twdT1Uy3%2Fgyjj9yv4K%2B3Vhdgm2%2BWoJXWrploFFvF2GoK8Q1KUqAi0t0hU5klMSbKJbEZ%2BYBGi69%2B0bv2wnjjHcmNcV0hgOIdc3M3GliqDvysq6P56Tt692jnpN9M0bZt5OkHWSAdC2G8g5t7DhFnpOZPCPdwefCYGWt0qOknA6mzYOGxqNJ%2Fphaeq7o9z1MCOx4wpnNGamd8hichGij%2BDuyCrJIZWz3p2iLH38%2BdW4IYO4n%2FVj4E9Ldeo4P8yox8Cqo8T2ndf6bGtCYqfiC16Orm11z3z8yPYXwZZxn3yTJgSO1nC4LG9lxBjcHSebn%2FcV8QfYGCRISlsA%2F2B2nogdg0dsKZmUdBk281jgUPAr56tv6RSjDt3a%2FGBjqnAd4t%2BQndTwFLz3DQ3c%2FX6GHRHp49G2yzV26AictYbLghcNhh%2FQ27Cwl0KlpwNMMsgJkYwGdKGzHRr6UUqRtQomZ86Q3w4U8wjD8uLTUiNgtYgqZ206Gq99FHAfnfRv2Z5f%2FyKpvGTEp9gIfP0rQt1UAYL42JWyt6eorauywo1Nm4%2BhbnyfDPMwq8%2FDLVHxtInxOrcOrqprU2cLMesWYuwuD6yMyLoG3A&X-Amz-Signature=f4922f1cd95b64d2dadb04b428900d981f8b6188e8b6e12ef0138914ba810441&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

