---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SD2PXNBI%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC%2BtVpCqAvIl6XSv5DfSFyxZCkapfXHfC20%2BTZu0HNlDAiEA8YPaLANk2YiUl27ciqGF%2FqO8zKq60wY%2FpTD4X6rbpOIqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE8KPXfigN75kCT4YCrcA1HNiE4EzPOE8HRikVzU3YPpk3%2BPl7CQh1LgLLSHNJRPxaY47aNzgYEkUNqW1N9GN4n7mKHBri37bOlnBrToG3tf7CtkoKYWFp6z%2B5bO1sIkyRHWkzjEJ6WQG4tTgVAE%2Fwz0uHBaD2R%2Fh0Ht91PyLVVixNybgweHBjmuJNvDsdO7xgQZja9TPfvyZ5rvu4bb%2FE3F%2FJFFprzyVoxzG7Cb5Z0DXWwj5x4UgMwkAn1Ey2s%2FJVbTCjoY0Y2wWW0PT%2BsS9%2BfhemnYnbQ0AZZKjBZW%2B%2BYiFvSk7%2BSyFJRHAZvgNAKaDWi2nrxOxBhMJ%2Bx6D92cd51uCDTAaBSbxhJbbTqRGAaX6RyvBmuXg6Q6Po5wIknmdVfV1oQQTqOgQ94WP0xpADqqQIPc6L2xDTIQSb7es9LVNnO8pdQZ0XJyVtjHymye%2FvfmvM7Fv0pSVL14XugY6XLmpvirNKt%2BZkTJNrpkobe72bGmHhl%2BjwqB39m0fXO3jXXVIZDROnG%2FJ9tOwQxghraQY1oR0dc46%2F6gjgdtuv822F9o2CnAi%2BJHMc2Kdeo2GEePlK3hrP%2F4XYPgBFZQ%2Fnjn08bw9TnDOnhkb3pvTvl6hnVeBA8Z%2F6YnPN9gD3%2B3ysupCg8s7kDaiht6MMiKxMcGOqUBm6F5aLBV4LBFYJ%2FH1YAMC0ZwfDg%2BAwBOPYaxfwcKpMYEIxLUfCcZLVFqmTz5YNGWKHx6q4C%2FDJj20qTEBuRPZUl9s1KokCXiIPxmAv3rGDTtLD%2BxtocWPt5iEB%2FeVw6elhVkDbTRN04ZRZ02sICOntzgFCWJRwD9lmIYTbhY4PL8cRktqhAX%2BaiQZ0PxfvhjzIQkfs%2FSUl%2B0TTxl5500PWLKNjAO&X-Amz-Signature=17d9687e571f1dd147205e69f93604e6969cddba485e6060687161542ac8199c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

