---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HL2VBTD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGFM7XeVEc2fO1f8pgW9%2F1hoJPq4bnnRsfWNQH9S%2BGD1AiBsOgRhoswlCBh%2FRWefq2A5ZXryEYgmI5BB3C0vtwqigiqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMD3A%2B2%2Ft8UPscYe7DKtwD%2Fynyc%2F7jw%2BRmrs3rb%2FJ8NwQ9ym%2F2Uuedw5UjyhIVmvjpx9e7%2B8vy7VG76s%2FW5eNwH4KzmmMcSElgd7n5t680tPkYgOuP4zrO74uy8F0CzXKkhsdycJJV%2FyK1JM7EeoQFISu2oWG0a1pQ32%2Fyi8CkpL6Ej%2B78nEJaFIz%2FMnxoqoKju7kZ7OklabRQvUERzpm9zKMmqABfjqiiqSmpw7ylzcxjlCES1KsRcjPkPiHO%2FGqIw121d0rbMgTbfRjdRLi1brcVaI4u0BQX0rT6EpCJjT7wIUWQ4qZTKabVUg%2BiM7XoAEzN1U%2FmUTmNOTVK0y3BtTHhoDa6Od0o%2BM4HPYdALgOhLakeh6evSsWwSlNlYRZrc%2Ba6qW7%2BEWJoq633wEbAvx%2Blm7fomhBnJwYONoYzOR%2BdmErEq%2BxvZ5yhRLGLZceGSWLo0TZR2%2BJie3Ba%2BO3FXQOhJTIpgBWD9IqKckjUcC5iRHKEjvFfB90ui%2FcvUtZJZugg9rEXwAPVTF5L66dbLkb6L70naOMsvYXWi0Q2nD5J9QigeSga3dY4852sarN0Jqg%2FvTaYqXa2wBbOEg6rnS%2BRjJ0qX%2FoEV4uJLvDMe09UCTlI7UPeS%2FyPz%2BHwXQ2ntiiggcfo%2BwAFcFgwsOnQxwY6pgFzyYg5eeqR12H9YJxp4H7FVW%2FFSYEqfL0xxRpMFC4x%2B5n4edqkY9fK3Tr75fXbhDnVu%2B%2BC%2BQPbd%2FQtYeJYOauKQKn3OfYuI0OIbgTwB3j1kCbrPqzBIHVYcyrk%2BDysp1CdzR%2B0B3jXkFzw%2B1mS9CjsFuYwy%2FxQFDmFeS1%2B%2BW8ujvHaruce3F6kYktvd%2FQgvlRxdAcnTZJ2mAD%2FjKK%2F4Pe6mbqQmevB&X-Amz-Signature=937bc99bd8dfb553ef67f8fde61e1404959f8d7ee98173d7274199cd63249501&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

