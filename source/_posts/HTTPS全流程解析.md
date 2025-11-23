---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WLEZLGXR%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGQZ5ljXW%2Bdky3Ep%2FUbSuugj6i651NzmfyTAg%2FAHOP8DAiEAn1sIQZVYX%2FyWi9EOzr%2BV%2Far%2BS8E58wd0pNcYYR%2BLVFkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDLwiF7nmbwvlYgb%2F6CrcA4QmNwhr94vesCvo8eT30Fao6rKxel8wtzngpaM%2FckGKDogdtC5HsMGxx8cUtuVMDp2tpLd0Ne%2B0wylF8UF3zbmM4ak2zl7gvOfD9pCM9djMKJxSYC%2FzqHtKGeOM5NhaszVSXYSt73Zo0UFftrLw2OFEjypTuX07GkkCW4tZyh424zJE1PNJoc5qWtl61nsYHmxzc2HghqeE%2FROGMfvIZ5exTqEggbV89AnpO09yjU9qz8rUq6wIUDXQkAIArEqQVYKr6Tksu2kaQS3%2BIS6VzqbdHfHLqTZwtrWFR0HH5OPp0TvH8xpzt9gVUlKbFFG4By%2FRWUmeqpvNagdw%2FXPAgP1eGvtzwwVtxcirVXAcILYHNwATs3xkhdNyzGPWq2CLCcB2lCRVxR%2FPLyQq53xLGZL%2F7CYzQASypecJD8v5R5XyfNBim6nNedAz3UTId%2BdXk7IIYbRDfkUHZyiUpm%2FtR%2B74iv4y2HGZvVSNgb9%2B32Y5jKz4DLeSeuH9E5j2ZvcFvgxz02a3I4Pmf5zdj6lm9O%2Fh9DFxHqQPSDrbhmynhUwvK8NFZYst%2B7Yb8ST5%2Bt1NLRHZjeQPxbi6%2FEANYYSPNOBwB1vJ8io4pKstyUOy6IpoxCKNeXuEirWZnHzPMOKXi8kGOqUBajhWuxZxWKkZWtclmIRE0Z%2FufYaJZMFjsYQFJA57HM%2F6X6%2B2oshTylucjvamjviXPIiFV6aDXlaH1q1GaWQn13%2FUNmoUrmZUp6lF8GtmALCH5FAsXIWuXDW%2F8QcNWQtPiU09Sze8QA%2FV7sSOQDiJw6QOc0kx9qwKG3ZqdF0MFqEeAyecr3WZdzETlVYqWNEp6nw80uHZCvYHkQk65gANwo7HS5N0&X-Amz-Signature=627a515b30f4a1caba869679175a82a758fac8da609f89a84bee16896d4bdb6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

