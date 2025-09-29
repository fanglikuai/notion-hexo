---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AOJI7ZG%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQDa9Nn6eBaAJ2QLCf%2FdN9z9J2m3EK7KLvaTm44tsAySdwIgPrilRDvl3OLngSOfw5%2BCQPSJ4qSLQwDTlagsB2tGtuUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPekwvHSJPisrbILIircA8KEl%2FuWZpFdrz%2BVAmIIJeP6%2FRBnVjyrWMckQVDIn1nBTUQIYCxad5Urf1Lg6rQgnk2mEXPAjePNwIm8qo7PIQAgwls5tAuW%2F6mst%2BFJ9qY0HOpe8rRilVvaTu2nQXL9QoyIt2iK9ETIG3nnZ0vXaUAp2cC9tfd6RqOYJsHO8il5ZmMaBsu%2F8u4yckraw30byXAX4wozEm5JjJZp6zeNlPa2RAOAr9hlRIxZ9g33rBXMm0gyBv9rMNeA9sM9Bm3L%2B6NNYs5tJUAHXqE6RAwTPl5WQRvcfWXp8pziprJ9jWZfG9fxXHdlcMAvVp8jTBUQ9iKu3sfrUIVPq9AyHO6sNYvmwLihY%2F1cKgQ2cuDESzi5%2BVvhbED6o5Fp9whH3rItk9%2Bo9dv665BzO%2FUdKUwMtW4yzlqekgEfNVI2CHH31XAXUdu%2FtfF519i%2BFIqafgT3SS%2F92d9U9llB1adP2KHXOTo4oJb2sScnQ57t22s7s3t81bY7HlIQy59GheTI5LIP%2FQtNZeFkoqBn55kCmhC33%2FeH5S4mRIwxgXI%2BQ6wWatF2VRGA%2FGTOnizUNkeRxp58f3ns8rG28Eldu7RFDsdJJUYGaMxZF%2FS%2FwY%2BL%2BuNjYf5VGrd8juX2cJuPuyNXMM7U6sYGOqUBZjTN5DAo%2F4S49nCD2LKjvcW7l1yBtxD%2FN4mtOtj0s%2BgPG8FdsTyWiYReHw%2Be4CywwwOVrItgBPXMrukZw2u1DOvO4BsbPtjoHPB2rx%2BCcPQwe8q9x3Jvici14aKHg76jn3K5X%2BQFG5hDaUNnLpBOXF1IuwGaaNl82OTV4Nq%2Bxvy8L5Bqxo59hQKrMqJjGDBb%2B4tW%2FEYwqNeipuJJ4TtcshFwBlZA&X-Amz-Signature=2ca25c658f6a3aa3bee4188611be95c00d1355c5aeb42282a9dc0d44b41881e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

