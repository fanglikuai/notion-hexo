---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVLHUW6C%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T190100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQDIHMsplWinLve56JTLiJC6LKP%2FtnjUJEqFs9KnujzEIAIgNIElJJvaePywQSGRsDNb5dOaJ%2BI4AJjPzFWeQgwzUvoqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW9cPhXP5GA5b2u%2FyrcA5S4wZ1IoQoa1WeljrxO5J8%2BXgv1eVeS1jgcywwokqrj4b5cTsOo9XMOBr4YwPilEdRC5m0QuQsJqv%2B5WuR5kbFOaeO4aVNB3b%2FWtyitggZADCamYuEL%2B1XeK1VxhtYOGRTP5teUi88VGkkaCN1zAFXjd%2BA1YOoqRLJfiE8Cs9rUVv6WVLsjPL6M%2Bzu3dsfThhRSEo6%2FcEEBUVw6hIDbZoqB8t6nnHhEljau6oeNWZ9Qo4ggWhuowU8lCaUH5Cn%2FBu7FHKSwk9odbwV%2BVJK0h55IrTtLBBoeiVi0xUY4let25Um2ENxxq6tGl8%2BRHFO7VK6zNj24hqLY0ZAd0Gi2OEZu4EQNdfTgIugr1BinJTiMpMYjO3zNWsxdiLdF8wSFJTHIpsqL6VwZQVC3M4ksZc080lslNvy2tFMFs5TCc%2BL%2BoXfNDFO5DX2Xa4qW98rLgqK74%2B8yaS%2Ffor2K7pejX650NUcXnKTbMsuvpE2cGFX1CgrOpZniC4q97JuCkopaP9CsjeMNgoKsRoxukiiVvA96ploQY3uMkRuYYbKqmo5L3Khhk4GOk9XWhoTdTlB9Xreh3JFoqLcJ2nALGUX2wdLp53raYa0QuQlHfxi0xJgb8fvnXJhQc%2F9eiyNUMNe38MYGOqUBZhkezkw1VB9mM5uXbeBeNXOmRO8W7TRFB6mOzfx9UiJyxc3Uh3hGAP%2FyuDdfIUVwattbYl2sIK0M1nEDwwpes%2FaXtmwE491jtHjt4erfQnM54e5APFmFZDJTJy%2BCDIlYMHofT4O4bAFYIVY8RmW1qkuI%2BWQOtarlYxiN8amcHBpSg74Pt6WPjExfGSYg1PqTdgIDZ9AA32u42UTQEqhZw7QDo05w&X-Amz-Signature=df450a0e6cffbc5c1e84fe6554f020e6f70b972cffcf578841c7cf3d5f33b371&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

