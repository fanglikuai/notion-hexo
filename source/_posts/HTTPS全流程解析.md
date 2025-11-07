---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY6SXT33%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7yzze6knNx9BfcEwBXbPgq%2BhAbGkgj4knixG1%2BnxyHQIhAKQE5s5ccohyAap0CnlDe6Z3d3%2BtxBCmwYUQ15aZ%2B%2ByUKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww5J2eyFxDTU7vPOcq3APltRwR0C7%2BFeqEjjDol1u7T0jA2LSO%2FOqIhuKv5ubBhJ5nk6yNJt07Ajd84XjuS8rBqWeC9JroSU4%2FgBXBkKqXc665RFjGFBn9Uo%2Bf4ajNDcyvImUPKHfx0iBfe7OxHALp2%2FOKDN%2B3JUHd5eJA94O2LGmYHQfSH596474iZME1AW4ZT9O6Q1AKwtziVBZm2nA%2BK4GZjPl3dZl8SYS9rWyI3n7TAsSBzB86B3jAn0BYP9pg6kDI5f1iWBQ02XPnzkptgHx0BwfHRk2pXJm8hENBZ%2BdsEUNv6gsG89wRml1l8ssP2dojkK97%2FYxQDQ5%2B8qeroX%2FF%2BY9RoWJmrswPyMnU88nReSuPVqHsCjawa5vA8%2BYbVmrhL9B2zTCbgZbNC8jXztNmaDvrnLC6%2ByTydQ320JhU7abDBqzNQvRS7NVcHo%2F2WJZYrxYUFZcmy4U4EMYpiow7c6Yf3DJ9PEHz1EgRKrPXcPlicz8UEfunGc0aNy9nqUhpYPoqnDA0mKew0iGSQAwxdKSD%2FiluBEpU1tG6eXFqkzxWNcB%2FZ0nRk9LWnUmPQSmxeKvd2hVARZPz25OHJSXD%2FbrIe0zenUsMu7AwJksr8OOnIqvSGUmhwXdoh9KHcNh%2B4p38EcCZ7TC%2F67bIBjqkAVH77C%2FAP91cuEHZVyEVOGNRThMbVHm941jJy9VOXdBwW1c34YdtF%2B%2BrvqFPWAbBzqBYfWDh6EWr60RfhBAi9JRCDpp7U%2FQ1C%2BI0QENjtm2DIG%2FkAG0r5Tetbmi07TK1PM1WSP6UKirvMExG0hZAUxiM54sANZtgpyqim7GVS%2FCYcH1WA4OUVaC8vfKPpAooSsgQPhlOLgfI8wn4tUsdKLIdcuaG&X-Amz-Signature=de0f404616d1a700827a6b5aa155b6b3121d93dbf4e8e76160bf9b5284bc28b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

