---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HLPMO3H%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T200036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2BXuAC%2F0s8uyHkkKYjU93Jg73M70FD0BcnbSrCw%2B6VwQIgZhSyUNrn8LQ8u24siqbw0vkGMPbt0WSYhyGZdANPoA4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfFgCvm9ODFQMJ1eSrcA%2FGquoeCeeGSr97u5T3ZjhsOelijCcrI98HvRLWKoaSqqjoRHK18If95MCWnD7NFdIwmkYOBzLx0eraiXh089rrLoBhrncG5sE2doATHWdeDvZ%2BQFW1gUWiBspDfp0M0H%2FYzeGLVi6M%2B%2Bw8ueOBq4Sy28mOmSldft%2FdlJATD0XfrRBhthg44aCcL2dNJuaQNOdtq%2FfNE97x0R7OpQrnMiz9Lw5Us4no%2B73E60%2FSTjigiZoxknX5l9VJIOIfcoa9NkP2qNBExgzVAUEFoDu%2BGK0rczd7dB9NHVP80Aj7X%2Fdunm%2BtbIpv8i0i91c1FkydpknBPQLRCttXmTd94l6XjHuwR9xZT5xNSfxNIYX0p4ct1g7tPf3XdfcGEOLbKpmzRZH6EVg5nj5K%2FF5YwM5MLRaCbxPM6WzOkVX2trgt4Rkfksyd%2BZIlbXdvz%2BKixMFXeZ5EKv3QN5gW7PDa6W%2FNlhgOSuysQUBZVC7ZCBEAmsxvcChwGs6sQmmEoOpS%2BX90ClRihXxhcCC19ey5h6xatirHEmbGgCRoSDgOoAZ2aI2ntbYsde5Hzq%2Fki%2FxTLesF%2F2%2BukvabHexBYQlpnEp0zm2btL1y%2BvR%2BEX49A6vu%2F%2FEFe4bq5aBNSkI%2B2NBEQMOKZ%2F8cGOqUByYToi2fiNnvvi2IDU80GJ47dGK0LCdlKQ1HrzS1GGXHoXTKQ9%2F1i4kOSkpk9mqCjhl8WYFs9KZeG5IZKOzw3Fc306YDByIQqS3jxWv27I6R4ahoTuh%2Bo5INQm3jidCXuWIHX6R66cOmyQCvAUGE9pGHzGXLRetK97axY0wJMzcKrB3Kxox%2BN3bQeVR4GeoI26AXa1t6UhR6z0m6BxGtwM1QMa6op&X-Amz-Signature=47928422f17d6ed6a1d1fe5a6b7cada6b8bd99c1505576d173c2cfc64d491a3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

