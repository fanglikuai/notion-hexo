---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLY727CG%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIANE5x%2FpCTwPlaktxD%2FSpMUaPal65Bt5p7yLU7MjcT7QAiBpOQ4sRtJ92GwN9JpfnzyvBZzWQ1sP9Ek5ojwim0fyoir%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIM60lcRx39CwZUyu9WKtwDW5elvCMtKXIudz0CudxRAt9VXtKXba3fXHLj1XhEUr7j5H%2F7pAUfwdy2onFoaz40d8Y7iuxk%2B7G49lh3UhSaCFcbZhn%2B6rSUMf8n4OQd5DBgnunSrTOlNdWAFXeV5lkyQqsJTqgNwmk8gZJROft9d5SujukaAHa%2Fk4s1xYP%2FUUTd7EKUcXao1XIogaFmX7sQMzt05socbgz0D48a0%2F19Yu2kDMvsCC7jjrSulNsuej8LlnXWuqi8NrsU1aS49fa5i8KimsFVFfkXB4tOxeBH03l7lW88g4uQOW0y67JH6icdjjDv6ywxfTtxhQywbM1DBay%2FwSsInFXSeMFrvARpLi3s8b8gJaTdQN%2FUXMeoTyky2yhVuR3v0I3ERWScF8FBPGNdz9Imxt%2BV%2F3aw9iaSiKIKc%2FqYBbXWGNpTIfksplJLflGf%2FoFYPglCZlj3dVFzYVaiBGwuNPF%2FEDFR6zck22ICzxJLA4YYbGzZpb9yqUcXwUfhuo54yM4kwEo9pIvXcBr2yE0sWX8bctipzOOXKYejfGuv3muNPHzQEoQoTwsU%2FzFJlYSmZK%2FUFDf5GCsok70EWhcniouNs0qSsZtTuy8HjjpFz5xHfcY6hX%2BvIloT%2BuWVNBrfW5IzPb4wtYOFyQY6pgFwqszT6rwc%2BO0or%2B5GXxgmDh3AX2l5mHph%2Ff4WxnXDquJorpk84pfVayULbVSzSYofrQeAlUbTgbi8b9QHtKZMQlzY%2FDB%2BZ8DLonUhguZoqGoRbabVQMlCxCRo1jdpIXr2FmjrS7n02uNPfMF%2BnPXaOrhKw0Uaq25Kt%2FPEO6v2lX7F06M8eLBXK7NmK0ZB%2FmaNERZx%2BunXQuawbgpE3C1ZDxYB%2BBVl&X-Amz-Signature=d2d273bb8b5f9e2ff94a1e8197e8b717ab2a1a2526b5e90d4161fc2f6c20db36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

