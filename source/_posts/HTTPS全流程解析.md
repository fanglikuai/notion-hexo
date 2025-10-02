---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBN7TV5G%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAi89HZTGGbnW8NQ3AkjQmqu9mLM969VXtBtQNVOIjLdAiEArkGu6ov6WBSkGwp89lz57IfkVUlAPcavvmz2u2D3iW4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDL69kUpvf6yima1bnSrcA%2BjEY9tqlE3JKlaWZhWBeG3%2FO7zSOUJnYYLKhboh3Q2rwMf%2FQN2Hols1Zt2ZGLhTEmXFNoRW84YntUIOsbmYLFC2feD84c%2FgqYDko2zz3iV6db%2FzMNFA1Fq%2BJMJkgM0naTsRDUjMP4VKG0UryBbldxlBf7Ba4uRcgYFMxlWkFUnrPfqo7OwMYT4IZ4raEBWVza%2Fm5u1ZxMIVbpoR%2B80TkHrg1K5NRP2YgMcLZg4lprCrQDPWvfW79LIjaASV66YaQBRHlGtMnp7uUyAJ6q1K5F%2B3Rj7yrY56AaFfsnBGRWbJiswZIk3AKdRWjic5lze7rxnjQNeVkqbbadTlA%2FtffwZn5G68%2BPWKEzKUgZ8wbGW2FeR%2BE3Mthfx2MbsbRFb%2B93QCT2EyrPCkpsoaHjPdIsiMEjkvIX%2Fw5UJesW4Bxb1PBLCweQ2caep7Ql2HdBGlVPl450Kx9CROCvqbq9K2zQgrMZDsjDOZ%2BINNPdejywLoU4Rea7rs1j5EbxrcwDL9DRwlKcS2ZkXTW7CGXxeBNAPp1uQwsUc9KRD02i%2FWurdRG9K6knuluNR9%2B4Jk4OD8SP6g0LBbX4kSxA1q1%2BkWJLGzUV3%2F4Z%2BIL9HKwYcQGKnN9OERCupWOs1nJW27MPva%2B8YGOqUBD7KaDKZXcp%2ByKhqTMLlL%2BRAKRTohcIcvtSYJG172g3agclBHUvo8ZrhU8CNRzkJ7y1pd%2FAH0%2FMUVzdRoIjebkrk3coeir6AjScrRGCr8AyTyaDF2w8seT%2BjGCi%2FAqW8p3hFK6JaC0dukSPXTmxGspSu9OT6mqvlN%2Bmnk8A9zVxk2%2F7v%2FZ5RfLC5NzfnXkJz6ZoibQjv5tMmUBCyWTBSE7Idy8nxT&X-Amz-Signature=19a1d56db1d910a89852a4cfeb3fd0b876b592d20014132af1bdcded838d68f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

