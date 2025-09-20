---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMCWMGXF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIFszqLMW0PLdIe8xolDZ8%2BUSCISfCV8uYx3KBzqyiTSgAiBUXAfv8JWGFJXCYRBq%2FDj7CviyTpmhsAlKmJ0toToL7yqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIQtraVSavlYORWKvKtwDXac1J0SlRH67jIbiFpA2AXsGiv8CyzJvoscopkedHLjA%2Fl9uqtGoei0lFhPhuIYRVNEFlxA6nFGF1I7HY0ZprNKqrnp8%2FqHZUntIEtKOfDFXwVXCTuFpfoGxM2TlxtdHbLor2CG%2FMfTzmNKoyYfw35d0mH0bEVNiRIHZIKZTG%2Be6II5JBIcoi7PalT%2BlgAzDZbbMyzRL7AvEQD6bVQMnU8tEHlczdv91O%2B0ipBVP%2FVS4vOx1bOzIzvQg2Fd4YbYuW%2FVdyetH%2B1U3fcHOVbNwRjxvA2xYvxPbq6vtbZridC%2FtcKG7ZMHCIEP2BcPnzsQP40LD3RF%2BOGCrWyAekTvUphgywPap9mkGzNvsctCHOOu20cWxRzxa6bUy%2ByvN%2BaGicBhq5o%2FScMEzuePwZadmAHFrhpPLRDkLlhAL5syuAUPc1xILF4jCRCWdc173txGCniMkiZtvACybxd%2FXB1ozum2VcuU%2B2OEEevXYnjWG%2Fdlds2za3X%2BN%2BpaoY%2FRygxgmd7Q0I7qBD5Y%2Bg9I9y3T6qf2UsfIomdQjl9pmB%2B%2BhvNIijzMZfRTRqWsZxaRmXE0R6HkU4LxUc4AtKA5FIUzaEd3bRDbvXHEw9vznTxIL7259%2BGWiqxM8nLX5amMw08u6xgY6pgHTQEzfwIpsfG1RSd0XgmOCNchvMv3EyfzvvptXUV5AsX5OQ3GToDSTxwyXLBORZKlmaGT6Ebz6sNIruSrGfGahwWCF94OkgeCzOL4ZCS8B3uh0n%2Buj3dpECZMO14Ht2zVRfd62%2BGqGMXFgxLMGkM68NV%2BbeWtRIk4soyp5S1lOcRwBnnMgORk8zA1KBTkzPbqrXKVAe1hUFokzt7lUez5OHdJE%2F0Wp&X-Amz-Signature=ee9fd0ffad90fbbad7c52ed3af2bc52682fc012dbf5ab0c49b2b397f30f2fb40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

