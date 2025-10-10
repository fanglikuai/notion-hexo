---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2X2K5HX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIGFurbuZL%2BpD%2Bd2FrpoIxnB4jE1wRcr6m6nA3PUV2xR4AiEAp5aGftoSTuNZEAxI4N4Y2wYz04BFkdj0VIpK6KmAmsgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8MQeGO9LFjPS1myCrcA92Zaq22WxuEpmDSSQsDsB8Jfsl%2BFsvLOiBAWWoFaeTHfgyVCR%2FutHubDL6LmElEVAhBg97qcPRI7v%2FR2AobNoOVqbr2G2lYdzdUFkBPgyEtULLo196pecU5aNlBUzmU6AS%2BuGixz5PElgdYbqMytsR2sGCnY6FZb6ag3HEuG3%2B2z1422vEWv3MwXHZmbnAY6O%2Frqag%2BCLdd%2F5mat2vBEyIweQvs77chJ9iKw3P4joG8xMHs2oPRceKleMbTRIMV%2BJN1bmE09mzzoCQXpPFZZ%2FjR4jmDZLOVgP5hZWG4ARaPkKYwEZ8yrAsD4gSjx4hS%2BtV397XAx4YosUom7fL2PcitNkBLOGd3BPS5U6QLF1KmNB4fzPjVmvdrH6WnRrc1rOi0ACn2S84XC9rn6znPra%2F6Uhqj8KUKXv8B0QDdniacs6vcBgVEfpxZ4Gz3Pm20ZXUI%2FjGeEC0Mb42bEfF3fvxmCWqcCSLZXZClozmajF%2Bl5KnKyOZc%2FEJdgbFxR2ysvSfxI1mo4VtrbO9hRMCrrncN97yg89590UKAzXB5jf%2FSnyPELEGwWi4ccUa18mC7lMcGjBCF7gVzMKlHErPLHipeKGofYLiePNudCuV6RMwyh24r5rCc47mbpjaxMPj6pMcGOqUBwFt73uiYCh%2BrI5LeZxuOnTCoaqther5JdKijImB8QRWbi%2FoM9KsGGeFt7GkjIWXrseR1ZQbYRJyH1259ShrYmYLOhFaWoNaQ90KJeS9S%2Bm21kPM0TJQxl%2FYOkb6137lLEDYV0IivGZjNqokopmv1Ntq0bnZGS42OUfxVwNLxRU0IBa%2FRvyOSTI27DCrHJCfLPxUdU0l305GC3HrJPdKFpvho%2BKrk&X-Amz-Signature=fb7316890f793f802ddc681e75e29246e858480d002f73a168d0d3ac233a0051&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

