---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3SANYLK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCIGegAY%2FJ4ODMC490UlFnzey1Ex3qzBTGn5P44Gb3uCqqAiB17W1wDBgkCqxoTdWkQPNX1jAyCThm2w5w1BAGaxsJCSr%2FAwg0EAAaDDYzNzQyMzE4MzgwNSIMiYuVjbTgv04kBTXJKtwD3GYvV5GsuDbUg6shKA6aEAWOgzJqAMtQXlc5U7bhM0Az530EtCGXQ5ZGSL5Y0MRJW9OTuaV%2BoacKhNDnmcAUnYhCqTBXZgmFrSskKsCfDNAZM9XIJS8VXt6fijaTPr1pMWPrfcNYFQBAg7V%2BTqTk%2F7s%2Bch1QBH0%2BI8ygt9RxFG0aXUUKYMLfFk3BpI%2Fg7FnPm2liS3r0pS1zSE4v7Y6k2IStgvsSlHjx5lOIL5s5i8u8KXBMfr0%2Be%2FhT4h5izmQYwFGB9ObVEOjmSL7H8gdxMSAjJsVFT%2FuzeHgL3JsHALTdGU3xJAzgwnz9P0IGM6QXcNO0wk7zqh%2FxvgSEjq1VCxu4ROJjLxQ68qtjm1TAEufJYfIByuvJPLjbDMA4DsAVo0FzPWyTtJ%2BbrKjE%2FsZP8jEvYlDhaJnnDIQQOECqxCywQOZzZc5vnScoeDlamc80gYwCuRMbwCZH4VlQRs3WTN3JO26Dr5rQLlm7nVjUc5%2B%2BwEq%2BJzFO2jM7ZRvBS5MEtyhC9jfUBwmm8sPc1b2%2FKmSIHCEQOZS90fJ3tP5Wfo246rjeP8XD64XiSqrqManeC7c58zXtMEa2CjoHAqzgrNgcU4gBRMtdHrok%2BdxjrrfClQSLheOnJdMfkTMwudDRyAY6pgGz1gWpKHsq39ysTCC2yE41iSejrNrJt4MHOh0DrCM69WQqMU6wQbHKXlQT3%2FglzFmljvUUBp21vadHv2Ko5rVdlEwK9uTEHl5P15phsRPWDDAJrL53kH1H9TOsix2bWNKUOS1fULoEzjWYyV4zCL%2F8oDAUhDcgs%2F1I6jP66Ot7M5GJeMULrRXimetZ7hOLTqjZbN%2F9oehcxtNLmmV%2BYYfKXig4l05F&X-Amz-Signature=d69ea0fd6202643555cf0bd0810b61e6021fe4ee3d5ce3287d325cd4d111d3ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

