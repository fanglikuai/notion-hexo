---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWIQ76JJ%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH8yrVEfbhSalEfDp8k8UCCwRUZtP1L64ftyKAYPGxkdAiAzWqa%2BWAg0ylCit8FNc7xykTqJ3ayipOmGKv07f2pwriqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP1XY%2Fum5S9fVQx6gKtwDj6PtNu3IjIkCnXV7mWs5ImbZkMAXVQt63kYBNkI2B1bRNW5gvT6RYvPL%2FAJRJms6WH0ESBfwdUga96Z2Bw08Tw97vjBcW8M7AbGP%2BnvDuJBZ6W2IKNFnK%2B5nzoBaaX1e9V%2B9qzF%2Br5oLqliEVgLw3EPMA4kJ2cdoMp1UUTEF%2FiZBwGn%2BJdT2mGX426xP6YLHOeAcrb0qI%2FB2BgcnE1zikAkrdLCkyPHM21mbRblWpQ4f6hZZIwLrUi9md8BruYfNbGcvie3EokM%2F06jPIkQxRT0Vl1NVMkmn36mRqrxRxnBSdfkJeMnuyEXn3NyHNeFv1RS961zAPmcVQKy0VzQWgF%2F6KEMiFrDYdEVo7s3fPY7aciIK0YOcBg%2BU%2Bg9cF55DEnH0DqwRgv6UZxgc2qsf01XTKY7wrrbz6U8I%2FMD49J%2Fib1WYMtyCNYSsR2aRNihm5Kk5cHllRPAwuEVBhP9eiW3mVOC4DJdwJ9nMTk%2FBagnC3WXg%2FHWwGxdpZIYCnasJoe%2FUitCBSmkkOFDF9Kn1ijrblVRlX%2FH1MXi81lMu6W8sFiDot%2BkJ9VkmG1CX93SXarRCTPBZs6EhuiqSg%2BHzLvvF8I5EHNSwLROYeBeeCs1oUlqU0Sf9ikKDoFgwspL9xwY6pgG%2BPQlJQ3erVcnaXlHYF7vazYczn%2BjLkqZ6Qwh3iHNPUbQeHVHS03eBXnGx6km92vdWsBJk4juc7S09zSHCvyUmGakKdknmRht1Q4WQQjYp2AY311W9vS88RaJA%2BriMOEjR5LuwqpLnP66g0Nilue35ZQ8cSjm%2F7W7oC2ZC%2BW%2BrTZmS5nPpexpniRSdoByJ9LJjKd62wHTAzHpANKLgU7kddyNHitOC&X-Amz-Signature=4fc50d651e7fc0b21b0e58a810267b1437cab78795e1437dba0baa2fa5e57376&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

