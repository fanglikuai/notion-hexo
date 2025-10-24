---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ACUSTSC%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC7tQHiUK7pC%2BC4MBiBX9qrbxtj2AV%2B0yNePvZhP9dkkAiBufBdsABck5nW3eN5RpnUgMSF9W7G41HJsGUwFeaeS5ir%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMZGc764a6V4PRcbC8KtwD1Cg%2BVs5ZAhIw4%2BlYqVSK%2FSnsmyszyUGHKF3Vp6LKQ1E5bYjgCT0vF3h%2FDlUzIp2pCPEmlxfwdQpVK1FSBj3Aa0NNeoChisGhGGvYK9k9TQm4Xw4eWKsF4vAZk%2BLU7N5sC6qFvGM1DfBx959iBIJW7GO3K%2BQ%2FiLTyOxDm35vQg0qm%2F6ToEAGMyx83Y7krMZwufMVPAuSqdGGaZaRTPqeWnmNIqEkFtnK%2BQG2Gcy9aHcYFMe7VrYHbWPoUr0E1zKqNYJMmUoT%2BucS8C%2F7YeA5XIMk00rCqRCt1RKrX7kJaLfeTNH94P0%2FGUhQ0twB3RMg%2FtrXb3NaNJYky33HOFMbZwZPVOR6vi4JRaThyx7AaKwavzJcmqyxYq8CtOxYOKIRMr%2B7o6MbpYjDdVcSlQsySjyllyCR%2Bszq%2BXJjt%2F8D0B0Ycm%2BxAPVy4sTkxIcnbBWTHhKIUOHYuHzCkyG7xTPncuI113cWWmcKad6nINWsY9a%2Feo3i108rPjLuX578d91eufGrnIHgtt%2F91yROUynsaPGVvt2L1vZBq4XWhfSpCMUq5ZxWgd%2B88xUEsAvO6ZnEcZrnkH9eFRm7CW5ybDhIIIUtOR7XSXYcWfAFVywepwtFOL43%2BNRUfvTktRsMwjMftxwY6pgFz10eIDl6DQzncsHFqyRsnpe3C3J2NOqyY82QbdwDn%2Fbt6MZOzdYAJfNVwvB7f7pld0iAIzgWNG8uCy6uYeoJ4gltM2yfJSPKTR9CH3PSgE4wEHvexb02mavqMnWb5RYx4CgZDf9W4mxVA7q5Nye%2F0iG423U0iCgrr0QN542btkLQmEaF73PzQbYvpwo1vjUcuRifVvg%2BfhWQDs2cRMn0ce2I4%2FLW8&X-Amz-Signature=43ed0415fa9ae13859f07315351d42e37e5caa64970698a63d1ee7712f361ae9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

