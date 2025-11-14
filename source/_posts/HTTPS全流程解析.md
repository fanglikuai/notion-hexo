---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6LN6RF2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T190343Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2rm70cHbEWSbRBqLubQufyqq6J70NdDtI2Y%2F%2BjWrooAIhAPoK5lffkC3WIe%2BDMtdiCGmLRK%2FrHDhKceIpNKMkCBpYKv8DCGwQABoMNjM3NDIzMTgzODA1IgyM6vcpvILk6z%2BpHRsq3AOxOYmDhN68F0AOcKcvNmcyD9pvxtmPRhnz9LRYWCIZnjEyqb69%2BDz5heogEJ%2BZkhb8c8ojBvpbrQKVNR3nLM%2BoaC8%2F1rzckIxj0Mo110CQcFaeYv8p%2FRjTGY5bp7urbPIWAyiMAarxjhBRlU02r3Ff6V5Sse4ulxDaAC54ifAbivsmlDG6ypYGV8%2BMNVXvrJpGXnnPSTLnGYKiqUTEycImb8zpQvlDW6bRplFHT7xeq3nYuoiqtNR6EOUxOG2fjHjDbQ6dDSJ2oCqT7fPzVR%2FCchBX1r5zDWualtDuD6xUaoclnq8Ny9o%2Fyv81WgyZeRMjwbPX8LGe1FfUZpXxalIkQ2pc5azK0JabhmRp6hTGLj%2Fvl97BZGuMq7%2F5cjxXEifj99Ol0LJEbWCdq71tygYU%2BaGJMRPS42MVooP1qHy%2B8FuyhYsGIURLmhDWS9f%2Bg568D1y7Ycmtoj6q8Xqiah05g%2BdotTX%2F6E3WOmz3IcN7XuTdv5Yrja8ubJOtk53uYwvA4Y65L3vS0FcHiNBVIKI9xyVQOyodlP4uAZ1hWCtvRoTsvEHt8v%2B5%2FdNETOROYYMnmzdOmP0%2B%2FFfvBTAxJkdmcKOrTWMoZD2VcMFN6rxV1CGN0m7xWgyRIiXBCjD68N3IBjqkAUrQ60ZUngWNNXnD%2BpD30zkA5RLhQIYKXwEyVrxSKubMOz7A0RqW%2BlMzOURRVzfbH18tl%2F9546Qy%2Btqf6rLQ0SLFQWxAMuPFcxf5SmDmDbf1D8gFXimHWrBtw4mkDYPi%2Fft6%2FphVIb6vJpvetlgl26yfjWN1ow1fU8Kcvd86T4g%2FPDjBy4CrDMM1PxU0k1vO5YFhcckvALzf8oUNp14BIaudxJJc&X-Amz-Signature=64e62465faecafda6e993ec5d91181ea6b890aec4f60be60ddc2df9d3a37db30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

