---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DII3ICX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICCgKQEs27Yap3f5vzAMA7qQtpfWusgbsojNxd2n02f%2FAiAaiOAGvEJPqB9AkcP3vTaAi2JpshaR4gJW%2BJ7qhqOyWyqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLHO6o50r7rPjj%2F7xKtwD0czDOsMwe%2B%2BPS20jaBhiOpRt1BcfkfRuL8OJpLTW%2FVmnKUJg7OxMJDbsEJWW10TQPFXn%2BxLo07HnDT12gOk2uEdwgfxbe8GZ%2F%2Bj%2F56olwrcJaEKG8SF7fgeatrW%2B5HWFr0bCYwH81qGspb4P6eIRoAQcKtVGkqQKcLbIbJetV4b1b22s3v1eg9e1mq2pFauk5NUOp7DJd8DYr%2BR3bLBuiG2vJCyUfzT7gQmEtkgxxf7N09G9vUEU57VlWr%2BT6pP%2BiQWYtn3k%2BDvr6Qz8LcxyEoWHF%2FsJcSFfFGipLKqS6tKzDHtKPsSUsLRxtDbMROuyFWppi2MRG61DjBopXCv6d7igwNJGnTGirgQBz8tWhR0UjrcDVZ4pMeGTT8fYGJQNjTunY%2FWIWsErG6Rp5ZEF6wuC7nfebY4n%2FssjLp5PtP3SU2KKUMYorXFW7ojqQZV3MwXY%2F3vLtx7gv2GrnP2l2uReDDKe3P%2BjTzfPyI7NBZoXWUr1SIgbHmNa7bETtPA8CtoUKhtSGTiLZjXhymwUQ3bDCYK0clV6zBO37%2FVjmWjzYkHFImn4EyJgZChDunVqpLeIN8F%2FRl%2B6j0FT%2Fq0Z7SGVPve%2Beyf2WeJZsBplZsm5MxooaCXGQ3bnnkEwrPyhxwY6pgHdpu%2B6MpAbt1jkjDfOUPhvOPKKobUbuldXy2fPysc%2B0%2BFJK%2F2vaGPEmT61wBnSP%2BRFEQ0EZkqHAmve4Nf8QlT7DSffTYoohJ1DC9dg24MhrxhogIOgTuKsgglZ20JWMwpDugCxJlq0vnPsJJU1Ojb79dKfESk8%2Bi%2Bnis9HU52N%2FEHUlzpyNAtCiwA0qH6yLmj1JmZZT2BmYZmLILqGAHef9%2FikoEGb&X-Amz-Signature=43c9d51f43f7c98a8f0f186252f0f3b8bdf7cd32a0e41cb20c326e13de59cb67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

