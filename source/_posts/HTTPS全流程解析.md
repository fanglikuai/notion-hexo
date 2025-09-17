---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VE5EXDVQ%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIBKwC97a%2BFyV1MwKc%2BoTggxOCRtcgWtUKEJPnxEMilNDAiByX1AaJZus6F1DdxLtJ0l9wV7e6yF4fvOSQowdZYk%2FDCqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FyPoTzBcm7MN10ZpKtwDHzEkjfvrOd7Ota2es79b43KKO%2F3FnDfsFJEfzqJxwjxy2S1xpfRTCdu51qLwtJvPTuNmcq8aiLO%2BPlevc91Kb1mXP7KFQNYShfRL%2Bu7WMMJCJQoW%2FUGEvqpfLAh5B0F%2FuNy2K5WNY0m9VZud9sv9Z5GQy282a%2FhpwO4VR6kTd7522Jf41XjSkipYI2eZcH1FKdScQA26vQOAOQU9RrE111SELvm3wXoi7B7c4rbnnmENSNPbXt9%2BM8IewmAAIpBZormD0U10YkanSVlTizrxWgwJxV%2BwxXLKrbiV3ZOfsYW1V%2BnwMz4prdwvXbewPRfnTJsRGBPKJRHgFa5Sn3yJd5j6DnzOnNsS6P9NPJ9OXiUVbLcJ3CTfsG3SBntOpy3ycYCIo8ecPGehk8rq2Jn2Nu0lb%2BagJnlvPlxyxg2Vgj7rHzmbjAibqmr3LBUlDuy%2BjDeRF3oqv4BoTWCiMAoVbSWm2F7QpWsLvRqFpDTJC%2FR%2FV7b9%2FFEogAehvzv%2BpVR5x2CZ98NB2%2BlYAmEAKHguWbQe9zIMMWNuhWPzRcNwmPk6GqEV5KcPVIrFO%2F%2BmTUINgG0%2Bfq2hPn5CLoVYXopfMmBUPO49N3JPuAUSHv3%2B84DY14GS2zCi1AOf%2F0kw%2BtSrxgY6pgE7g%2BGwyQ8mieGacKq2SjuvvEI0yRSVd3WX0JlsPE%2FL%2B2oZXjUA1QUE5rCRa3Xrjj6vTNT5qsZgQy3rB2mEhd0qShKGUsmCgaYfQ0CQIn9UTGBnIWLPleg%2BgtQuEtWi3Z4Uyhah1e%2BFGVHBur%2B4MSXl393tb1QCnUZS0iJvYF7kXNMseBUjSdQfGq5r1upJ9uHfrLCrv63sv8bD2DUdf3SpcZcgrZoz&X-Amz-Signature=2c409455d6176d2e3b8fba09ff51cda4a0ff732ff9dd4f392e6d289925344fdd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

