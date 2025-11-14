---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YMBBQTO%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDokX1KLwBIJrEFf3E0Zm3mw1P4XBkt5abX0xakiHGjtAiA%2BicDvvnAd0uiuuvcjkGDwHT6izK3aJpzvolv5we2Q9yr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMdmWT1hoNevaWbf5BKtwDSTeei0f3nhrBmOv4j16EBIwJ%2B2Q3q%2FS0%2Fen6E46bPJdHmrxQqgzS7xZTkDizFYnuA4gnv06Bgf6aSntTqGxuC3VDeEXR%2BWOLqx7Qt%2BSYBvRU005r98eYXeC8LPRDTELoV8g96jUJLs88mgmkjS15VJkAf85bTBhMZuh7ODMlyvICgBR9XcPpOmoNGxIhMlao84u4kSwivQPl4nxTZGZcam5ndS2Fp8UhjgmPf5OZ%2FfgZNITta61lm0%2Bnvv4sPqdfhWEBm7HykN8%2F4WnK9uAoXQysph7I9q3KksCh0Eo9818FzXqKbcFnwzj4U6gGEjYY3OOQZpYZopi0ZCLwUZJIWTBHNAq3sniEneo0WY%2FY3YgdIrm0CT1Ur3u8EIMR5esY51syTBxujQ54Xmey9jaC1VvOYcwUYqwcTWHz8zAUtZ0cmxJLqHwBVomUdRg8%2B0LglNJuAOkipnMp%2FWAA9edcv1CBxMNBO%2FwZunhRPO04I663k5XQ%2BMdezZyplWuvA6d3QD73L8r4xHw%2F53WorjQUpSKw8vZf3QF95Wq67Qji9Dv6pclGGp%2B13s61qEUMDCP1R3drRJ3Xp%2BOKfFKHyTZQz1%2FIex72PNotWDQgY0CxnkI7pafN6P7aUVktrbIw1ZDdyAY6pgG7DhReogXs8tkmrvwarC%2Bm0hzH0Kr0Y6Z6%2FsvixXXal1Kr9hQL9O1fPZv5%2FhvnDK3CJ93jyWshGgeIuqGys6nNm4HXj1%2FxApaMrS902Lx%2B9NPyqDytxWNI95cCxtEGS2xlfv%2B%2F2V59bqOe5ofTns9Ck5b06KHaU9SS5GsgtQKiOduP%2BBJfqcldR0ohWJQZdGK%2Fc%2BqbYM4bs%2BOAkixRz8HocTobJQea&X-Amz-Signature=6c7c2e428df1ca2fa0976be6b2b582ac21f0d1f88c51d27c73c757063de514be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

