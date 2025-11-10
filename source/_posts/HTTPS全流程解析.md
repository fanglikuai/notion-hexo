---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EE52REB%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIFaErhx4Yf8nZrhUtoFHtxdiq58TTDkEVlRjrvzEu9JGAiBQ4NC5w6Px9Qi7Y43VskLMtH12w1gHA16IGdRtazMQECqIBAj3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3T83upY0x9fYejfNKtwDh5X1bkJkkTa419Ckkvp97orXW2lPLAhsb%2BVraM1muWsFRpLiEzDLOnsd%2FGWoP14QOoWzW8kVJ8nEQ2ZeYKMCeLNPzDd7h3M8Y8zhjVfE9aBUjytGMoYgvmR%2FoHVa1%2F2KCQwZUTVpD7LFMx3sQ7uPfMyYnM2zvxO5z050sB8M6l9FP4MdJrqTZp2h3nUieX1zoCHEzw5YB1dxrd%2BkkNIH2KZUXD8xZWaLlVEZ8OmEzZtgQiKlBzaptYSL%2F4a%2BIbAX4HtjSNsrK3rs1YjJ%2FeEJCn0H8YC9LQIhkigupAmFaF%2BBOmdSuCgIgqAHt7gSMMhxO919cK38sbOyri%2FmlN3Lv3HCZ32eAd69V4I5kSTLQi5pdvWSZBoyVyF4TsxRvFOPtVw78FyufYgOInQL7WES0oGW127rc1K7zpD9Chg3Ixq0TyxnO3%2FCYlsLNJ5tyL0jRgRhTkEBidf0DMZxkZwrXSzhKP0bJwXQcAMcfQD8VlYefCwXlh6aEcy%2BSYqSKKm%2BW1uTFC1fICS58h0OoSiPEiAuBJU9%2BZaPrsW5P92U8o8WvDv7GZx8yaLUuGsO2KgOHtM0erAdT897xSn6s3GOTYt5d6TRaG2LpQbzNCCUTdvfIULQnzE4PgrIIWswmaDEyAY6pgHLOjmcRKEzY5Z%2B6MqTB7MLtmzUpt%2FZdu3a9Z00ODUjFQ%2FZAEGfAVNzmA0%2FxiYqFwZll08uFKSGz0asu6fDhr%2FoujFMzKpOOvqSqqP9MnceWL6l7Voy88LaP4jH2UFfL%2F3RK%2FUrW7EU%2Bjuk%2BgMJXwTYk1Rh%2BfasCM63z4Bss3d2Aign3VZh57jWQN3HBvgxKzE4rC2h%2BWYEKSdtRTpTt18R4BmTZ8Uj&X-Amz-Signature=02d4b86a8482ce67eecafcc91b29e69c0c88574d2ffff68a3dcab1fae50a3538&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

