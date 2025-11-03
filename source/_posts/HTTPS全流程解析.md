---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466553NW4HO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9VpQvNWO2C0olY%2FBYo95SDb6E0JFflHhOkK6sLL9d5wIgSV%2B0E1Ekqc9Xf5GMXkERBrO5RPZRGtiBkEmC%2BetTQzkq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDO791ziZTKIwLe4hkyrcA8JjtqmzzX9psUivECqrsrktVne8eIRb8gRwBeuwHr9l8c6oaxTkJ5TEmIHbzKpQpIxa5NByp99ppqZk%2F9JV1d0WKWgvEAZLgafUxW9t%2BVTLtPSQ5e1%2Fke8HjtRBU%2Bs5l9S01MLM%2BtFO%2BU%2BNC35n3gaCfGQQXMjgY0WveyN9E1szSZYwSpvx1vsAFcGWJZ0zFJQYmWzM9e%2FzTHhGAhARZ4CpOUV7Ahf9NIDgS0tDsBgGnrw%2F%2BxTkoZaZy1r3Hq6raSMO0XIoW0EbZ%2BIaaoB5DhzqmHmyBg3T8sfroqMqHvwkmKw4d6TlkQiEihaH6UcTwHUnr%2BvKEm1LRG%2FbB%2F0P5gB2Uriwdo6WQ%2BTHNasUxgoLxZDA%2BWVRu6MTYWOuLOq8L24%2Fky6HEYtbg8qW3BM7OjJ6Hz6S%2B0TaJsx1m8P8X69iUUO01%2FK8QNaStsbkfereV5PUc%2BhAU9BDJc%2Bjt84XweokQL%2FEYhnNJ%2BNy50xURX6YNFBeFAGBUPIhC%2B%2FtRuZBZxALlTZvxtuKVwCWWx%2BSZ1F7oN4hq1W8xdMZ9MYMTrQQ5hFX6UjP1TpDMRjressXOFvQ93XbtU5OUihYdVuOfJNJXV3l%2FzrTQCQPUAeHfy1YtKlP7FFFrA1MxON6MNjyn8gGOqUBkzyCWc%2Br0JjHYY%2FKOdhk3lncO7TDtmXgb%2Fy0R09mooPfA8uDkXGkxsTMSKIdxNGNdwm3N4q%2FwsGLwWtLx0WzvaT87yVc47B%2FIbPjlvuXn2hqBrx7KKSO%2F6717qDQbwEZwqoqiTOXkRZ05JSI9%2F%2Fbm8vvEwX5%2FJYjldfdo%2FZmtEMAFcTrqDnHTZ0gQJxoft2Jf12Vgy6qjUDitrToxyAMC4x2txDf&X-Amz-Signature=b011cc77fc6c707237853989f5197058de1fe03e109c49648e065b7ed48cb34f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

