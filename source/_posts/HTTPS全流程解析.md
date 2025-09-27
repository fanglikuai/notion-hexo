---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WZS45AD%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJGMEQCIFPP%2F9GNrzHd0VwtLFC0%2FnRuukCzzYXtkkq4R5ulPY6qAiALRGLq8dRT1jnL99wBrD342nlCQnAz1UD5aOTfnW%2B%2BcCqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMd%2F%2FcPwTLefgEZVtdKtwDevb%2Fw7WJYDzqqX3l3hSl5tXJpQHsRMJatu5BnDN9WPubx7UBcV4IH42S%2BsYaEfQCYeieujNjpqoxBksjLhd%2BM6UHCv7SN9pxnvN4D%2Fca%2F2TSgR0a6oPD8MeOzepFclsy6Gii%2FyRhQ1D8VU0BImdX8fwXtvwihrGSRJDy3NfTjezGHYuE%2Fj3%2FMHAWut%2F755DXhzq5ybfrtG3QcAtA11a1JQAwyO70n2X4L5pNHbv018nXDU9XP9Cf4bCFIEXPylf7VOi2chgA%2F8qRob%2FKzuGc3tJMarMeIviaoVUXUqf0H7whH3etOCgjjKM1RCkVgv3suJoY1Pvuq7Zu9trSfiWKXGyEzN0O%2FOgVicM%2FPnvhJimBCqMLEIqzm4BR%2B0Xxxouy6X5Rg%2BmllUhHFOlUltkVopGqBBQ7NW0T%2Ft%2BG0j5E1q5mr44oa9NNhIPx8KBBE3dNd6Wgu%2BN5O1qb62aioZ0SWDnYbPFaG%2FTBUV%2FJds%2Fw53TktzznwuG0V3dUfdLTiOUa1ZkI1hpYUbmbq4EMPD9Lbz%2BuNJ29JPRgH%2FdxjtnnBRCk5Pf%2BgkGZBGaRexixpxg7kP5loMEEYv6s5gLpyqZU3J%2FiOTyoCJVyvoaKRYS1b4mlQ4RqCafMjJL4kEMw2o%2FdxgY6pgHrIycb7nokf9eoAG9vqWPV5uCsgtf76bVVftwjxa8IW8IOrZzCDCL1zlObXJOFTSZVyasErmipk9WunAw3Vkeu1X4axUgylT7Bu7OKijZhd6TnTDbLiaddoXNfE3NuAho31eQUfq2JWq3ctF3m4cgcN208OR01SV5zmFQGsSx8PYBcSfk3p6o4%2BLerGBNu3Tz4pGFqhoRt6%2Bsf2x%2FLsV1SDuzcWDt%2B&X-Amz-Signature=543d020847aaea26c416a892bf3fba7939d369a275047aeb45723e47a7b5ce40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

