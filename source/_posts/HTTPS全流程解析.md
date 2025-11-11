---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IGJHWF2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIGqrbsTqAD0FHJ8C41jaW2E2zdEanJQTukmqA6lwNiK6AiBbwRObZYn3ZGEbuRxIqrBpG0RwkZVo1MkbBJBrkewXgyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMSomKtL85yDrftt9EKtwDZOX1ulUkRh%2B5kde%2BBbQdAHqeB10jb6hvG5ilheeMz8nbMzzKFMVKOxkP%2BeqENmW94EMd3IRb7LUBKVKobHwiouj85CQul12%2By69mj1FohtZ9dj90nz31bFZfvhqK9NNYQR86BmYulFsvZfubxvOmuwVvNpKLnueujfVqnaPZJ9tC48hiuwxXwCy%2FmWaHDNNMXsAHeOZdbk%2B6JDQZ9JlRdBPRF%2BjEWKJA3K3FcRv3CLKWYM9UN0NVSjE4apew25rKni8n9NOoQk4Ln%2B4n0yvueGBRBQsxkO1Pypj3GbUMzCHlGdqx9BFjywh0Y7zZWseWryZjD2GNx3Xn3SjWG6mYGdFaTTrS6Zv6xDCgkZDmrf9oVaQ1mB28l10b4m4cUn1mUmbCKwwP6vzPZL9zSgBV7eKAWhPTyQpjrijwjtL78AgZiWNAlKt9E4cc8IuKaqPrX8dVBVb0B6pbFtHDqAi4xWjc12g003LP2be4CeBDMS33DI3kS%2BuAzLfUBfem1IH4ptPfzTg7lYLfPzZnBK%2BvNth0v0AohANMFlnkwjB1PLZXP%2FhEZ0iajHYqdD9aF2pH05sRYDRipJaGArl8c6BFV%2BxNr4TcqrihdyOZThQWKV%2Ft8ZOB1jzEZNDaMJAws5zMyAY6pgHSbWEC6TwAjtU1sUQpiK6vCE4J3RE6WrtLW4KgWjwHQcrmzSkrTqd2JJZdGUu19GfLX8kSaTYFMELjlWTJB5OrODQhIiBCY5%2BOdV1hGitYImaCnDIh0ETqVrNI3MV%2FbRyujdFqQ%2BqZB8oJRI5FVN%2FejxISsPyFTxGd2OGDs6WgvFW6AS1bsM7506p65BpUaFKmwaKLGcTGpciFRktT4pmKXQA9IAmO&X-Amz-Signature=6de6fbbfda162a8b56b4d3c5f67229e42df8b2130764d847f9adc6ed2277fd3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

