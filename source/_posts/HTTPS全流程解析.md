---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOAO6SSB%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFR7tMqMnwA7M0YdOOj8a4XpyOtFdCBN15xyOEfvWzHoAiAfRqfNoLN9QkGlVldbutNFDHl6bYNLEPNPFn5p71msvSr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIM4tG4PQgMnUYs4CY2KtwDvBIz9NvOANP4ncToc0UX7Pmo5SPyRBcJsNsFrhvHNvLtH7%2BWY4c7KOksAGVxgp7CAIcqmhf9wnWvA7V9H0LKs00C8euF2UAewDo6X0DveMJ%2BuHD1%2FUYYC2cahVYsgExOEpFwJxHtdponnO%2FpYjELBFgMxLEUwTiteb%2BTQjKQyDjmemPa38Utq9ROD0XcU5nV72YohbtQsDjTCrFH51tgIhe1q2d9INpOYpqlckpcqqZHilTdH9XW5%2Fk7E8Z03BObftDLPvQVGRaScKOBYpGa5QyWVDfp5nXV9uCzKneY%2F1ubnlvi6ehE2OK4ML6azVG7Q5fFOI8zLjoWkg1KrSwHI8ehy2fYpmm3aJGD9YHrixbstafCJzV8xrDZh91wfP7RzW5NoSARAKLO4XcBmW%2BlV6zh9w4RSHoX9cPtJuf33SCs3eLP%2BeWvxN9qJpSFn1VH1iHt53pOke62PdgSniirHHrWudKRc%2BJPZlm3nzrxrxUfpV6jYLNgQX0%2B4mOdccGrLXQG8hmFZwv8UrutlwKTaxexWmIvoL8bzvpdZB%2FtSPltp4BSPGXl09KQDzzBacW2M3h0%2FX1yH4Az3ivSDUKLldg3Pyvs2DH6ih7tamXF5h5nUjPjfHAwQ6W5Z7EwrOmQyQY6pgGRvF0Mom3xm9F6c8%2FV56jmbk7cdeJGH2lspIdJ6zHp5bz8rsxw%2Fhyv%2BXp%2BPIFnhK9VtxVcyXPD%2BphuA0TgeQcpNX6FdUmaUoTjySftuB4ITJUdwExJFX9%2B6zRcHAyL2keFrvJfhXTs7AzQ0R9ND3f1kRHRtcExsx%2FO3a0tF%2FosC9cvsU%2B5%2FRxASEdqE4td6tVVm2i7l%2FVAyPFDChh6ZDs14TnQY7td&X-Amz-Signature=639ab96319f19a34e9bd97056f495e8b183500c389004d5f160385b951cac267&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

