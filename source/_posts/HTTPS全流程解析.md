---
categories: 整理输出
tags:
  - https
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRQAACLK%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T122452Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQ%2BFOLYs0aPlOPi8ecRMaDgPQkCrQTjYzCbq%2BurhL3zgIgaYd3cipiNZ94Oz9gKHUbQyiCoBTXSFkWzQoAsVkOcioq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDH1bODz7vKLoXslXZCrcA%2FjImH1uaVOjP0AR16avnPDMw1JcfaEFbMHgZlKSnyyI9r6ZZ89qOi4fVhm9DC5arodLzMMOGTuFU1MuBX0SM87y0GYN5jtvJUctpG%2BQMZksRFR9wBZExkHl5jio66giBmWgrPlQvTIc1CzOa6GDt%2BReBt51qs3Y5ZgTsCDWpkYsYODfY397jvGsZwA0NfMAvQROhViO9psTazse5vdsWbS%2FQIbMMb2m0A9NKEDDfCNGXYY%2FO3CCJa%2FPEbsCDN%2FPBzYC059Ops%2FDMm5i6gOhMWUM4dcoOnCPowGPEaPEim4W2r3CKJJ4zBzHlQ%2Bsh%2Fhx2GyAOXMrG07AqqO1qDIvdWHASDWoqnsiaVET997xEngXVyceE%2FgPFoOd8SD3inE%2F4T0ulqUep1AIrN3zTHaM27Ar8ny7GByIdaqmFlH5xeFFnonBB%2FGa4N8JOS0EOfJoEnFpTyR8Pr1BnwJ8UK5cI1%2BOU%2Ftbs6IG8f4PuSfIZERz4FK1fGOKse6cSZfpMd5sLFA%2FAlfDoO27xdJG9RB6QvNu2ZgX1%2BoTxJd99H3vRI2CUCEFpwj3rzKLqwimHV9fAZlFqM%2FwcjxrR4xHxcAOpkZVIIT%2Bv9E3rwf8X1gm6bY9yMmNKb3qcueyDjjOMOPPmcYGOqUBblc0zawETW83YxvJYlBS270j6p%2F8ZjSrN486qIFJAyoJ%2FLx8%2BXpCBLAKogCDaKmgWnXSi1r9aBC6A6%2F3267kmUPyx2b4KX6wdTey9ASNdsJbWCctEkLZi28LWwWYE29lDG2m3g4KgAvfKdv%2BbWJYezcWeYdiW%2FBgvX%2FT5t%2FlszCPVRDiOfdz0mloL6BO0E%2FchfB%2Bo%2FZaMesf2OHUpQ5cHYmZTiJq&X-Amz-Signature=b4702de2ccf459bb2d394c9ad6d81ec447da80f8407c09086c0d371d4f06306f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

