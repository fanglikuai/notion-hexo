---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDERM4RD%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCD8sSP9AmoeY2funDgBBQt2Rx1q73A3g%2FAdMnen5aLMQIgA%2BL3RuqmDQPBponygLVTdqLWr5WxXHurbFiCrHS7eWYqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJpjBZWK0SBDc3eCmyrcA90gTOfclXoznIiLTBmKxRA0QIzwEYmZtTNItQTPInRYJdeXcir4g0mKmNoM9Z%2ByrFLd7B7zVji1xDXuLXdpgWhguU52DBzCCToZJyUnJbQeIbSb%2F5rsfdGNWucCgpKJkCJIs8W3HK9t8bsUyqSQmjDWbq9eR%2F6r4oaYdOrL4KQMvtb%2B9AXTOihZegkdSS8wIb%2FSb6TkqpLo6ELG4asasbpzbybta%2FukKE%2FvuEQmYrUc7wKBUhGFJVJQQ80g%2BNhSfliVJdsdjt1zMvF8T%2FIycWPR3YXDTSLaYbc91%2BoMu2Gh0NnpCT3RyKW5w8UWJuamy5myl2c5zTv2KAviuH%2BGyhMlZWskdlh65usz40dA7mT1WQpHQcL4vzFycVQCap6le0hCMCpEnyOocW3TkbygYTA%2BtSHsAdp6%2FzS7M9Vt88A0bxjZ6syUCXFRpQR3FXVRzta7CmF5LnqLjWWzPIUOI4liKX2rkhrmci3xjlvpEfMcm16yptZWSW6vdXlEaoIZexqo2oHEjyFOII0y3kzQeeMdjlNuRVXl4dy4aKRv037LUeBquE8X2H4BNpPNh1VBIDadnzmhmj1rhWKSfLxBFHQ1PZzhz%2B5YmgY49MXOirkIor0FF%2FhbHla6nk1eMOKJ7cYGOqUBchdPy8HbAqVaFfarYQpCaOt5L8PrVo4yQz%2BDTc8ofsHFxg5j%2BLN3j6TrDb4uEagS87zYukKAq2VpM%2FN456J8ZpHnkTdBf2nHIWfYMhVOJDhL5BH%2FmJCSc3lDrGXTMe6tk8XAtMIMDN09ARA9MSWv%2F7xzvKMh2m4tyxr2Y8%2BcX7D%2FOvijwfnQP6%2FNdeTWrNYFkvgZr1VXqmzGY%2B%2FRTBCgrb%2BZrM5u&X-Amz-Signature=ba3700d6595802e4b65d1a64b76a995d6e7712b2de88faf03bdbe8f7bf2cc8ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

