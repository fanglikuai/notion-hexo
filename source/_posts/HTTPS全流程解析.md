---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AVW4OHE%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T070100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDv2%2Fyek8DON%2BN6J5%2Fz1bXEKu1V7OQ5UOPXSt56VyIGmwIgCf1wWVfwc%2F%2FxAdkaYFIVJ9C9pLTOubArYt309khUoKEqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOSVhYxWJU6q1Fz5FircA53hvJm0RZ%2Fa%2FUT3JCPLtfhapYLjalOsqDKeB8FwrD6MgUmgbls3Ng5MHaAdYALm4nrI%2BNjRRzDS4q7KDYlrE1Q9hmbGJXxEdQrZvsT5tyLwTSPCeSqa%2FlNxv%2B6LnzHQ98RJ8%2B0rbuAjd5ipgc7evZWxJG8owJ83s5A0LGA7snxIHJ7sCqrcKinJ6P2O6CLHvuyjPYBBwp295zOtpGZaC7BY9YWPTYTMZlQGitp9fxAbnIqAn2UeEDFkwbKhM4Ej37iAKlWGW8xtXFLpTnY6v2W7z0HRlIhtTQlflmSIFuQqPpIak%2F%2B%2Fe0xu1YP0XElr9v7NOdEA6TMCglYZUVWVXUTYiM7Pmo9FMDTzIT7KGQCsaFzEsbVsxbkEkJB%2FciVgXOaRDjjwLwH%2B02cEFXKN7mAXYtcLmj3gOC%2BAVZm95Ok16CLzu3vXVFwU0kyPQTXvxwVze93JtXxTNCOx%2B8z37H6cVWEVtF4s1zXeJAGOhNaQtWvGAi8nDlXmyU0tFXKvrp2tQilTJW%2BlO%2F9tac86nin6Rh8mLNbvf4Bmeb2h0fvpCZGavhNrkoou1beP%2FxSvEueKCMCRtm2sL9WkA6yTsCmRRLlvKWc2jONOOFW%2FSMnr1i90W7rELFxO3gnRMIDaoscGOqUBR7ZygwP3Alq1FacTXD8%2BzRFPRP28ahA6PqBCQjiIQMAN8yKCgJ1Gk523onbhd3GaR0Qw18pNGbr9zo9kRh2BfuJi5gNsLvUyDM%2B8Hmb4860BzYBMHlIbCZRzUtxFp4cz64umEtqfZks9iFHsrruqjnQ%2FMZBFy0brQhjdsKo8KZo%2BgHK2aT5tgJrzGhnmbF8Bg5c6jWgI8wO55QX74y7aW4niBbhs&X-Amz-Signature=936ab7c264e9af65a568b55c7505e531e77e3780bbd10e7a744e2d9872b1df62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

