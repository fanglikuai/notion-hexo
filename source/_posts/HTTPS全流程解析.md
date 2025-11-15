---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ONPCSGH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfMOE2OKz4RlwVabLr1OP%2F%2BG1vnFkAsd3T9b2RQcdIXwIgEl9VL3SQxcqtZ7Yae1Enzlw%2F3uPUCW2g%2F8aKPg5hZwYq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDAmRxRk3g68qRU2t9CrcA%2BVpMsnlbsdzh6FbVVNuzERkm8nY7hy%2FJ5y937l9VrtTAJGQ9TvuKkGlAj42iRuX4OUFbwyEOb2E%2FafSzzN456e9RnSpYj9w%2BKT%2BgG45t3p3ld37fMxMRJcBp4uakuZ8KleQUQ9RhSgfTilr0PiSyaSEGqT4uVqpFxOpKvgwIo%2FFo4ek2msCHzI3BZFDHKc5yFHVYXQ%2FZmdyvbZ2BRd1PuRjBvO0ntpums2p1aMVPUqDc%2B%2FJ7EbE6H%2Fb%2FhYnJjX53pjrSOm9Q6Mg9c0YYwJ5dDNdcR%2F7%2FxDJ3%2BcCBi0Rf%2BFwsjYHaGETSE61pnAx8BHIJm2jwvMAiQeIJP8ekFNB6jvVD%2Bi0HuH0hnIPqW91XPVS09Gtbc9eVYkkAtpKa%2F6ohu7Yqdu3gP1bQ2vo1ykvWUe5aVjOMrVibhdnD8fTYaAapJAYPfjgSPZEO1bIlXCNdEzCZG6AkJdDirJKtwbnmjNC84oJKlMooMHt%2BLLS8Dzr1s%2F34iW9hUa2AxduRAYv7ivgQ6D9jc0GDUVmf7TrTkaRRUnM4ShiKqPvi%2BI3QrqhQ2DqBmOiIBL%2BcLcl2wK8x7sb2aoRoazjJvNqbJA%2Blz9ff8f1cQms%2BSQVyI%2FhsfQ3d1M3fjJp2QKNqIcQMNOE4MgGOqUBVD1YxENr28LfrMm6VHhhKUO%2Fg0xuXIqRsedbd26qGyOzi77VY%2Fq4qEl2%2BK3ZctcEicj7h7HMZV8xPnQZ8geQ86emdPW8PNXkQBxKcXnOZdZLy5buNDipcSOYX05NOlYOZZBv0CvzcFxFXb9sAPNlKi%2Ba2D1QgBexEPBnwqwnXXSYxijrVtGXD28oEhVAN%2Buh0Vh1OjoJwu8EMgkzf36uAvBd6i%2By&X-Amz-Signature=ca8bcb431b2b89cb80d90eb2adab09c5a7985bdcd64247259a2826f917bce8b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

