---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WW4P36NG%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQDpyjcx2fOwgBxFt%2B5NBsN5g5ds8OFMIZjm9pSh0fDoxwIhAO5qzFoXreHUdTcBF03%2F2%2F1zWffK2ZGnbUX0O1qGg7hkKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx8Q7d8E9RPdtlgo%2FUq3AMwUDk2TWq8FKVIi9HInALS6Ib5Mid2R%2FoSiodi8n0PazTxwsfX6ApFSe8LGZ0FXcttYOJ0VwB7jOyAif1UkUTefjNVreSXKuHCzKVR5wSfp%2FgHOB6FjrEl4zpyz7ueoSUH60uy3OxOEw6VC3hJVgonuzAIJc0tZE%2BnAfqWTj6Vyfyqi1y0yeDTKKG1%2BlmQFFuyWvCUlnfgKdVn0qCXXtu1lOZoCvzuIHJmZzCnYXuAXBqrSgDKFJxEHyX1hBAkhatdP0qiKmIKyd1OlAHUjyY%2FB4DDPnu3Rsbi57pT%2BLkeyKws9JuxWOxUAi%2F61%2FnQj66pT54ZT3ybNbcYBNOHzS%2BtkZDcjRzB%2FvXkHo%2FjlOQdjiWQr6Rw8wUInK%2FijL8c1JNuPA8b2j%2F1M6H2Yzun90WqxDmQablCrnqEiwfqhA%2BogWrKgQFer%2FctLaXWoCG3B5MvUYOT37kpxOyE0iHkZtEmv7exfni1hKxy3XM6qy2dGq89%2FzjmK5wEb1%2BA5GCdafnDB2KTB0XlsfqZIZQ6aRb0v3l8PdF4LLdvL1j9pfzo%2FG61fH7wTVM4LWrnFE9usFpQCRdllsAcvlV6dizNc9xcXJFax9nXp9Y0VrKKscQQFQY9BnOtYa9a7Z1sxjDsiu%2FGBjqkAVt3xXflobQl910OzdV6PY2Mh2ooG4CfH%2BABIODNueXHuPZze9z3J0AtlSqtu%2BeyBEkZkKLymB1feS%2Bq2vVIkMrMQUqiJ3n15cxIqf76B7TijfsHY8Sn4xkMO2JFzX5Qdo6nE4IVrDBpagEeZnlbtitNqW3%2BUT8zxp7wlxZJU2eI38%2FxNsPdozEWmXd69wuaiQodi6amITdp%2FJsMrX3xHSWSwPbh&X-Amz-Signature=6ec4c719d856e302f98853e207a29e53abb89a0a466be62801a67a29a62b38ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

