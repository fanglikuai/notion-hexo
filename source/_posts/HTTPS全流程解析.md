---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFAZZ5TT%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIHfNnwsVvnLglcXBC9ZOW7Q3aRlteTTAQubWrci9i%2FN4AiEAj38M%2BQaYQRLVywLKZzxmchkQkIAXui3mv%2FSGc7tp478q%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDIDPzLXd%2BXAiXumi9ircAwSmOvYFryQKAd0vmLA47s0kbi47KlnR6glf9zf6WulD0%2BqptMliGyFNMrjDfha%2BpQr1eHTQx5Zf1oot9g1LSenYWmUcXdo54PtUMKL38rbzQq1qgnq0SCrfUanzZ5RYXUn0OoXDIbZOHX2GV8%2FxRrAOtJT0bube4QdhNQXuZafVsrN95x98SSPGqH2gPVGkz6fi0%2BYgBEs7FeA%2BmBdW3lL7fJWb1gzJcxpu8Jpbw6SpJoQl2lcKYQZf2KxJdnaFElbT2BHUl4s50oOBDMCWgs5I4xHR5BAThcMFr63%2FCafn2YGzsuY89Up4nPGQEDL2oHrMv8iLHgfdBPQSe%2FinRuUL34RDnzD36g%2BC6ZddT0YCTnOx4EXZqmK7SBoWA%2BDE6FrgeNSFOHOkVQCB5jE4Uov03ztv7081WwPDa%2FC1zEjtEu4faNuDM2b3%2BEFtpWHc3twKpoWbzu%2FjVNrHZBwvmqSYDGZRBpoyq7TKi9jNNjXcZYMXkLwB0jqETWY9s93RKuUEXe%2BSHHZrhKMtKaMAnk96%2Bfk8TrcgVMUwwBKFscXRxKi2cVH87qdOt%2BHC7IwbEJObERyHCXGjEhZ4ZMKfzfFk23g4kMYbPHWC74z0cjktorSKH5Zv07ZO83mtMMqq%2F8gGOqUBoWVdYHM3povh3j9sJ77m8%2Bt%2Bb6X6UATD2M%2B5F0UJqfmsjWHqWCuj%2FXcID4fDvIfSkR9YW49Cwbk66sKBnLAcSr296fHbRrPskBMYt2Rfbo4PqV090S6nMQZxF74EgQRVs6ECocRfTwn93Wlt0ck%2BELDqSF12fWTmSjQTuwrk1JrDpi4iXVRVAL5zx62APD5yDBrpvZD5gmhFSKDU%2FH358A06Uy1M&X-Amz-Signature=f1a1d601d1017784102c2dcc47b5664e2e390e9d249e66cfffd6492727d0b227&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

