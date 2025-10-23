---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637LCW6XU%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCY0zLbp2GH2xBz6wmrUL5f9lYLVR8JnjEswe0HBNyongIgbzpxNjGGQp6DB2p9%2FWFSlkUH62Vcbz9tw7rbmUSIBpMq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAAwn8hUaRmRZyfONyrcA54DiLLoeD0GufOP8M%2F8moEz%2Bhi5uTaMJZTxLatx19Vnyo%2BlmtcyWB%2BrjHVaCnJ06dqexdbRwxwdOG5MAlkdobz9fsyGg8Y1zHa52qxFpDosVYe%2BLpAhGQR9R91A7Mati6%2F8aXz9ie9BxZKcrGM2n4B1v%2Bm4ODLWlT4kkU1IVU0YCKwPtUYIfu59I%2BxFfFZr6AIO7vFQ%2FN1OKJvV1biZpa2yylvGSw1CLCQDtvNgj7NQn0Rvq7xg%2BjrqjlDNGKrS6Pngxp6cHAcps48KlJBU0vCxMGso301OQ4EbXTT8AFih%2FV2Un04iOMGTceDU19PyK%2FNI%2B1L5fws%2BaCtHSxaF79owQYs1OwlmypfSBSgsTlfQBKa3p9dhqYgXe4CSMzZtyk4g%2BEBCx1f6MVh4NPSlRTwCkKX5OtTEdBg79OPF4%2Bd6UeBW0BtlrkNtfueK4cn%2FhOXxTBafJP9p2fxw9Lpo6EpvmrvInLtxsU2WsiJF1%2BHIfs8RktiENwmcVM9%2FQdWzhYHDL%2BNG%2FyqVNR8cRXCCwIQhexa8GbVfSeiEg9vC%2B3MWSQNs74GZ6Bbj0DFvY81Yir9y%2F0rPsnv0Tq0Igfp0pxmzZXfPNBPUOVD%2BU3FTF04x6SdGK7XTzFJ4nVm4MMOr58cGOqUBugxNovA6DX6T8fHm5Lf2dnfF2oB6sKNR9SjlTig%2FWjc49ndsFcGrvwdfDyT6lnXsj5o4q6MhDeBv00CfuC73urBIIPld9Qvwrcm5A45F3P6z9VFEUA2nXvOyad8BSTWPsBAPYdcYKPzHE9H62iv6nJeAeu3MBAzoOEPzQkKgkQYUB2Pz2uOQXyo7GFUSyULphqyTWfLIowuUElpteuHJzRp96gEo&X-Amz-Signature=457580e6786bbba6bc5cec63dd466728c3b5f3481a7aa26775d538f35d2b71c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

