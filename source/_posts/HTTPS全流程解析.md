---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J4FDKHB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T180056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIGj%2F3w9t%2F5nMieRMOILcj7c%2Fs5UjUFYmS18AB1%2FuiwI1AiA44xyBkeBkX0IaRBMeuVHZC6qM21HYbydCrk%2FI32gLXiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FLOc%2BYsiUTURiUoBKtwD5j1OuE5dtwBSZZKOtwkTS8nbQCQIP9AJNxA7WsTqtxgKva7n2QbFwHv8oZkq0gYPZWf19LhMfKDuL69qvS9WWbXP80bcaK30feFRlc%2FT9djg7OJyYmWR%2FYFUqSA7qs7rQXS%2FEvcVwOePqFk9Sk6qBBsXmYWq2E00O9B1njCAQrvvc%2BA2npsHFsJgvc8V3lTWUzJO1bG8qsCDuxHaqcYOGdTmJMny9mgqAEakpPIaWAXG3Jkng9FjBfBRd1Tt1TmkagXEKcxNZG8JymrEE0HI04a3tzGRwnIR8kwBjQCs%2BNinUNPXlT1xFhoGv8LFurAjgW6dDS0d7e5c9Qvxu2n1e1qIPhnuD%2BLrBwb4K%2FsqzBCkzKj2%2BbH%2B%2FUMoKOPGGtjErO5huVvpScsRurDIpMNpLlzm9C8%2Bxng37siAmBdVg1OouYWl8M24vwe6B1z3wWWRxAI%2Bjw32JHtZKFmUglAhNcGzXnZoGBPtaPbBHysASTZ1vSROtarJ3h63JQF7HcUv3QvJdhozYgywqmV%2FWnPkgIji9XYh%2F1%2BIT%2BjAsK2Qqb90Fai4XoPERSQoKVYeHHt0lsws5iwWcaciYPytwgehMkaPxWfC6bVLlU6vaMkLtciKYDFCd05TYu8s%2B48wyfzJxwY6pgEXcbSHy56bxLCpbrCxhLtES5bxlx6VWawDMdS5X6nMtK9NHQS9kVxE2j4rbdfM7iUIWVaW5%2BGjrWQ%2FIhAXdyg48HD3XR75LC8smY%2FLFJj3haYB7G0jJ6WzHLpFeQaC53QwaR9V22p9f68ysp6Y5xfHz0ZiqFnwh5koHjStGbuTGNGPAxFrt5ykSKTBkX7o1t59mnOONef5l197WpHH8Yx8dmQ2gQGW&X-Amz-Signature=35b98e65199dd2d186316eb4eca29ebe54452285614a17a8d8daeb284c62a7c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

