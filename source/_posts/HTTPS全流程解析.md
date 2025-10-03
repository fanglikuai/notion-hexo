---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKGNCVB3%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH48UClHIk9o6uLn9aftRK5WVaiicLx37Czph7UWKZ%2FAAiBMgnMBiZo0MTgLWMNqZ7yj0qMYBHbnI22EKgOeGBi29yr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIM%2BziwFahvKvXSMJqdKtwD%2BMRl8iCT%2FIvOhP9yaGMiS5wXvoabG31TYAkSwHTw7PsQycb97mUbMGmJdQkBXcRdfjZOPvntTL1vTXEgx08j1aMG5%2B9bx94YA55HLWOsvX6Xe6gu1x12eDyisbFfqJ23tikr1NCrE6lBLb0m31BV5zKJM8gFwz2Bxy1XORhz8yqrQYz4%2FGNs4%2F2UYvBQqsnqR3K13mXyggZyZE6AHBt67g0zR5z70Aq3hP2C1XAdvM7ee4Ee8l7Fn2uBeVAVFb9js7L1ATWbKsxjJWOynSsuzjW42HCyWmiRg7BErLTVipYmqUg89qe8YitvG1L9TnfjiytDy7q5Pp0enyS7m%2BtWi1X7j%2B7OUb0iDnJu42m8jF2Cvj64ehBef8aPwUgbBdp%2Fa%2BTG0Zbb8XvR8rrFkdH%2BYPWLorZoQGxtgPfJEeN5yUnbPKD6pc8n1XiEGF6cBJb89sKlxPWd0HhyzrmqsWNDBBlE%2FQAJy60i6O3KOAyrAm5%2FAfaCH5hYlxJ0y%2F68%2BEKsgijqP1Dne0JtV0HVN6mKVgJWRrWgNqP4br0OU%2BTbW3DccwzUqi7sgghPOANb0Bpi7AzfaXQAz%2FtDg%2Bq4L0V%2Fh9IJ2NHJN5QPwdTsz3C7OwWDqjD67bEUWL8hcA0wiK39xgY6pgFEEiSpmz%2F03yddonFMt2tl0n%2B4%2FhrdXRb5NhqsEWVmGhaAuwzFV7OshlIyovzyOSMsteLT%2F2HSUkzz84VLXJzbPvV1AooW0KQW7KY0bPBE6THpf5F7Rg3KzQ2GyvT8F14gwPlv%2Fpwuul48ywgs0hN03JIPZwG%2B13rj4ZIZp1C2tRNi%2B5yZ2vLbD5MPlkROm%2FY3taqC0%2FmE7mKtGbIh1bv%2FeclI3tMi&X-Amz-Signature=cae0f62dc7b1163c2a8442f666bb8a8be3211384d3136bbea014063cfba609f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

