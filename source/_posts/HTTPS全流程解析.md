---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YAYPT2ZV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQDhy3AVM%2BoxdNfJNGqCZc1M2ogqiABo0xEFlIXN2Z4hXQIgPxmlmA9l0uND2sGw2JxWFx7TSZcWI9P24xjoPvaeJskqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPbLDmnAIEfgJZ3%2FQSrcA1eU5CFIIkus44CwpvlcEicSlii5UjPiwwxGbW9BvyT1msuk4EUfrPOf%2FW9tEiI87UT%2FQ8j8oGAa2RU6zrsJCSfs9GwtllgmYiMwqBfNtYtM7EpAFy1WVAtb0fjQoNzgNNttx0j8MRS7zC8hCTqnle9U%2FWDX6ti6u9STRw1GZn6Z9ndOeAW3gG1xrlMn1JP85ErIwdC38lH4NrbuJZ7xSx59PTGbtbip4VQcl%2FXoF%2B565Bc3%2Bqf68Rb9y8MPcByyc6zIMBxtmxQOiTUXRsZwum9qdn%2FGhX1VAootr%2FzxU%2FNfxEoTqpQKlEi4xuxtlonUaOLY12BkdZT8cBMykBxA3LiYA%2BJE1NIqV0QhBew0dYWgqy92SbzeqPqzF6g71KqTTkPBbEnbb%2BjWmiVy%2FIsnJdlYK5WjXF6ZVGqo13crmrXsT0nvtiUT1zmZ9iHYH2Gz20x8c%2BJttXABEJh%2BVyuYSVckcwrEj8zl7TDskSKHJhhlObkP5%2BVc77jE65dLfxEft4rQfm%2BwsmnE7Mum%2B6FZy%2Bp%2FtRyreuRWVy3%2Bb18ACWfmFoFnZGXpCx8SxYnB9Y63K4OBSeNNCDC8kIxllwS8HLBeVH6M36zXGvjcmbuG4mmhU%2FINTp2px6iqREcTMK%2FqucYGOqUBCCB%2BqfuCywSGIHu22sH4FEf2JPoEAf0oRyNReigASJh5jyYYoFB2q0SERuyJNnIk4T9zKGjj3ycT46E3mY6Q6ehiKJjl1cTxycebFxYHshioqneozlC3cHNSvr2%2Bnh6q6YOeDreHSpQ2zuXAleA%2F1M%2B6lsjgWcHomJHsS6FntUj848n14FK0RgB0g30zUwDLwqSZMPKdTxUXwg6F9nM6VsW15x7U&X-Amz-Signature=efc3d79957052b952961a83905d7448d9abb520659cb6c7cc33cff1b1fdc6c3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

