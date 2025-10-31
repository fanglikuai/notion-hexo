---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKMPRNBH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQDgHmn3DFp2lIpGzxYKKKkU3M6xY1e1aawM%2FM347aOIGwIgT22jhMCYcaQgv7yJ17cifgkS0Rq3v4sPZl60kdu0LWMq%2FwMIFxAAGgw2Mzc0MjMxODM4MDUiDOwNc11D9bvs%2Ffpc4CrcA%2FlnYWloswKgZD6OFYMzDlNOIjPF%2FKai37AnDTSern1l6pgTgpvFNZrPs3yREqOmS3yuQrSjY4Gyt9PDqJ77jpyLkoSkmM4agOR7QzklmnweitE2i2haX9lEGE2wqQxxXzzAz0oTzrJ%2F0xn82HPStw2X%2BdzAXgqRXe2WV1PejEZB6H0vsc%2FJlht%2Bsqrmz1WQZ7VzvdQpR5oCjDV%2Fx4RPvfhNHYjyiExfbSGYhtAF4P5wi%2Bb0hrgEvRLz10LD9mJ2%2FY9YaUpJ6x4Bsr3arLkrr00NV0gPhRGd49qr2MOFJct3eNEOUsuPjz4rZRpBXM%2F70Yxha7qvKTAa6OdItLu0F2dPzyPNFpzmMOQkgA8Br45K9Ri9DdHUzPwjTLfaOfhhfN1DrsZnEWJHiemmMwB5NKfFF%2BVNG3J%2FyLmpcLGaSm2XXmApkC%2FkDWhTzXrpONqKe10GqDZOq9sr%2BmcVskKaEEvZZwlxqN0E5eYMNpO5bJuRM4bYA3uzIVgfkxHfPGW4f20m56dqftcA3k76rDOdHx2Hw%2BNn66HeoTAjVCbmzgRMVGqX%2BXxQtgNSm8F7F3xNFRL7%2BDioTK0NuuVWhTvjDTwWlKbmNR95znC1Zz%2BTjpLgwP8KmdaMeQyIAH6mMPD6ksgGOqUBU%2F2MuUvx%2B6%2F6HRpG6vbh6a0%2FDzS1d5WWpbX4mI5PUQdeC%2FeIRVzs4uWnrZY%2BdjHag1M55Ih3MFM9Y0K6CEEnDAHgjRmpPhxKtS133IpO%2BgMWVtB%2FwIoLt1mNOQkbuO%2B%2BFlwSoNffE2UPhD94X9mn8bmT2lqCIS%2FpdGUhaiNVkm%2BzuoFsz%2FlzdqO1JR3OvcDR0pxPlwv95WkSYhbKGp9Rh0InhFGi&X-Amz-Signature=e8ccebecd568d0af5c0a1ee14af87c1573ab546edc48f0591946376c353d0726&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

