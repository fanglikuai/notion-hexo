---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WOKOIOM%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T120052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIERf%2F2EG6br%2F%2BlnAJ4Nwwe4fA5Esuz%2FrUBFRYvBobN4hAiBDPJ4z0dT6Fs2pLkq9yrK7ErY8kcoxTl%2F7nCz3AE8s5Cr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMPRIeDcd23%2F3hSaApKtwD7tq4H2TyaYIrOOYBtAq7%2BsOLDG45fFOzpJgL3M7lH5DrDwuVf5wdezS0fcHZ0YaB%2Bl9Pdnd31vJWYWeSpQ6aU4aZ%2BGzOIvblVNWBmmwpXlqYIiX2UjDhlDAk9i%2B3QQedqDn3yFDt4mu2fqiHYd9%2BCiO4cOz4Ww3EUjHdTcNXsIVj5WQBlErSozpDp5Jp98UeGX2C2usWM0TQnunrEtgY0cR%2F5klpFsGJ%2BWqKiMKw7iDu%2Fgp9rRUQ91UCKX7%2FcHaSxRjPv%2F%2Bk43dcvisfaqrgAZLhF69P4TKxwnNK4wENjmuYhDGCxCQHaYnLmPje12XY4j0Bn1NvFiFKhxEXh0uJqQvEw3bv2K%2FNrXCyOYvFfcPmqcTeeREdGdTpQc5Lj42AVETHq5LPXjqSnJqLcQI0zXtve92B5Jmo6QqivYHfV2BKT%2FW1ZtF1X8chYh6VP4uPkhG%2BBADaUyVdyGNCz0vrk%2BNXz4u2%2B4yXss%2BwHTw7f84Fg8ccVUC17BKY9BY30dPtkSRNw42MhpLgtIjAt%2FHalnwqvh5ju1Sy9dyg0rgMC%2FXnGcsUMZo7AwMz%2BB3Ok7NotvfkmmLeQOcbqBe5gYg67QEObtstcx2NJVhMOIjQIbbb%2BolUdLpBM4VEkosw2IapxwY6pgHOzXd1gdtZjdvE4MXJLDqj8oKqqjZn8VSdXK8swrCo1CEtHjzWFaFYTM%2BjjKhmFS73rD4kFzww5co1OyDbN6zBtFtJgpGmB0yWcAggj9L%2ByldOrkgl5fxYlzInUFKN8E1plzt%2FbuWwM9pj1hHjboZ8teBVbCzSsLhb2Pb%2BDRwmK9Vs10UhSC639D7Oc6Wktntiw85bVT%2Ba6ZWca3lhWS%2F2Qopxh1ka&X-Amz-Signature=c20527a7121d1f7a253005f0904091531702a85b834e55fbc6dc87cc58e2ad74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

