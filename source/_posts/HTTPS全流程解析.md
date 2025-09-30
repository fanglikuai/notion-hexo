---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTPUKETF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCICXVA%2B2ZSXbrUYz0u0rByrqljINFJ%2FZrUNdO74e6dj%2FBAiA2f%2FaNUJ0aVqPPw0RMJhuKPAIqdnd6qU32xP%2FAB4TwWCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHUIc5okV8%2BUsdjnLKtwDi4qTkWb6AwbxK805ibqUcttC4o9UgceMXKfwshrpWaPT%2FmN150p%2FnXGY92gTJETXJuLA65jRc70hRSk5M4sMdGdZ2WwdmNm8aq4QLlH%2BKF%2FH1feforNwcL4UjSZEGcjgSvbJ4o5oC05g8wXPK2%2FGcNEgIsQnfGpmskoxT1KL%2BzJmd7hfJ%2BmESP0aU1FooaoyjglDJ2y%2FWHsecaesw8vckQ7bP7WBc7G48wb1gpwjdOfkuID0r9w1AUY%2B0L3Kl%2FWlzOPI5WJyFl4kgKddFGTDDWSdNLjGERfqiIZFK02bAdQCLGfB3mA6vJF4V3w%2Fp6xN9HQtg%2FfD4eQTpL7u2o%2FauTqn25wp%2B4oZIK1pLr3fYVeSkgYiWzlKTLLb9SyfEwFWDUcz%2BD5prznWJJbEZSa52MDYhD2gieUaCcdIL3holnCV2IWkP7W69OGwrj3kWaJc8YD4h0fkkP2Op%2FWG0Si7KJYEVB8nRMyv37GIzCj9mXszN3ZwzPO4UB5EcdTuPV6zZJEDUCPVvPCPTE1EwA4zjVfDNxpizvZSOpV2JzH57xIUnoa3ppS%2FBV0D7e3%2FPncOKd4j0iOmqKwNnQTMzFgGzZtE03If52g%2FWIBXks9LIKXkQS0B%2BdPtgae3Aqwwt8jsxgY6pgF5ykxfuI9U1JehHWoqHsEzxlXUI4ooSsNGCLFQ1%2FMz6LR9KIlzqMzIVbqCRsPZqT%2F7jqahwX8bfD2%2BF6rTVBiQlBryjVmTD4GbHwFfpBVeC0SI7NNBarfWNyAQOvZMkbnMdAgAHWQVnssctPRPCWYL%2BwdfLY%2FUyvZmfV%2BN2Wlza4tZXpE2jrD5FVnfgil%2BDbMBfHBuHL7aw%2FV4dy%2B5J7i4J0L7kqy2&X-Amz-Signature=07e80183002a4e146e8af28a14ea05e894b56309ccc0776405ea1b329b41d675&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

