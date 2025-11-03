---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5I6ZIXD%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkADk0KKqvs2wnToCi%2FKm%2FRpSJfkXpgdZn8xsQunOzpQIhAKaQ0JWncJiu%2BAliPLcxMK1CMkM%2BahtFwlIfsOK7O%2F9AKv8DCGUQABoMNjM3NDIzMTgzODA1IgyZUHXeSgt1UgCmigkq3AOgOKuN5%2BhFJ0GXJISXQYfYnlvKGV0WIUIAlVpR8Df7Ld5dn0U%2BfuHkv8G%2BhE4QMjF72OPx2Yl6Xr4RJP7FaiIz0cIrYtcWZfYyUWfK7RX7gs%2FWPrvwmghm%2Fry8qVAf1b8xcJ7xR9h%2FT8E3u2n5aUUy5c0raKjU0J8CGckUPsNmnQAJjN7b%2BTTfoQAb%2BGREXE5iCxQlDI01NfVUP2UkAXFvSLMZVreyUKCSzrwODDULOS8mMNnyvJNYp6VOcHson2K2TCVfWvfluCAeNhclW50nyn0xvKM7gGIh0trBQXc%2F4N3CET9R4vh6w6VGB5QXpxn5EdRgcvI3r9AkZf6w0RSfHk8DtaNPTLihAFqQ2xZhtOHg%2BLinrwHPNkK760KRNynz00YqhridXbTBAvZq%2FqqEPV90gG4Q5tAY%2Bvt%2Br8qlr7K9RKs%2B0xNAWuD%2BDp3EFwFHCvmp8aGd59NRSVGxoHj27RJYZ3uzPYTHuBPXfijGpy7p28o2%2BiITeOI%2BtQofcganiV3AMZ9c7uYKh8GlCEcVKG7CRkbP5sYFG6jccGyu%2B5V8x4xWeif6prtse%2BoSAOuJ6NjOvPKBZZIrZOXE4BdlXojAvEYIWksz07kt9R969e0cxkxqxNb%2BckpdIDCEk6TIBjqkAcKGOMv7rjMrewlFmA2rqzSi7zqNSavZ92OmPJ%2FWKN%2BwFOqzdPGVKlgAlCU1Pta55gyQGPsCyVajc%2FK07ihUyPrbr0jjzEz9LfQ9Yxu5TNSx1I2iJz8avoltb8esgSVTCBRlFDcp2n2j%2Fc%2BKrDqLwXzMs%2F0mJeZqlUSA9cpYTKtKbswaV6hsPVnsTvpg1H2bupPT2OEHM8JHfEX7oAAI8oxxf4q2&X-Amz-Signature=a67fc7944c787cd1889a2ee61e7b66b02efb1779e3e65516692eee1ac92f7bcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

