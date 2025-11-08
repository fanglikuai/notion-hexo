---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2V7VSSI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T040212Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCICJLiI7BMkbk9zQaDVHkexC74GlR2CxOA2B6j450H0ABAiA1yRrw3gA3ZpK9Jc1DYWf2E5TSQqswmqN6xuEOT2DljyqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv9JrUjymcu8lABx%2FKtwDmJIeZix%2BYKPTs3H%2BQ25vYLXqxVbSXlEnM%2F2t0ld6fjAfr4KKPMK4J7uXGAcG%2BXHEcCLkvtpE7%2FwUGkImaft1ykGEzOVSNStbyf%2Bkzt4ZZzszlKSm9gT8trE8iPFw1ZltXtNHJH1geVvcxMQJtBgTDB7aOinQuFvL6D4L%2F%2BDis0npQkYkPUeDJP%2BmPkpWvFxlVnIb%2FfRO6K2%2F3c8%2Fl46VTeORRIZb2%2FoFsGjVJq96a5dzbtexHuVJqaMwKd89ZjMkimDKKOcH1fZTLsDVXvw3WuwBT4Y0EIEPRiUcvKIpz%2BLhDa2rpVvTX%2Fki%2FOVjNG6IQAbW5rb8rW1apTYTf51lm1rinodfNBeqMbnmmtgQUrEn0O3uAcv4cV2P4apWizhwHUoF%2BM1Totqe5tqpiKKAFk49s19pxxu7UeRic3hfEAhtprlUdEMXVxxJaPl%2F3m8xE3uhv%2Fbp0xE5JYY5B5ki%2F0mS15Gd%2BsbQNBQMqyxEoyF04knaOLVbeMZBn00AQw53oHjPVitEp5vYqXkCTtKBWQB6XnSr6pG51wZzwkvxcAlnCSE5rDmt82mKXjXCzL6XQvXo2UUZkU88q9eXYaqUbJKD%2B2rxr81zBcr3NBZJx4EoQuptsLazq3Qd2xwwifS6yAY6pgEHCF%2F4QoMXuYzP2EOp%2BBgpmTRwH7AOHvYL%2Bgo3ki19LgiMdv2sP4Pw72f8Yj7LNp7tW2u4RjsSYfOf4DQTwxNnY1%2B%2FAHqERIUeZ5yc0IFOh%2BUlugApQJsPy%2B5C%2Fuj8UiULrETdGgWNZR%2FvjSDjylzqwPULEuafSaajGkPJdi68QY2hu6DP%2BegSjS2bknkuJ211r8sThSmqTp2Ey1NWJ8Dxff6Z%2BIGJ&X-Amz-Signature=d1e2c4b75b5144596baad9b20d5bc3b09c686e203d50c15f7b7c82a596b2deaa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

