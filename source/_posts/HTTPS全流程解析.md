---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTUBM77G%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJIMEYCIQCM%2FI9ARE%2FZJ5GH5IeNcSJpj1OD1ILnmfnc%2FOgVyhS1EwIhALqgP%2BOPlys1%2Fa9GIG01CiWFUMI%2Feh1iBHGCUsgSn1C6KogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyh4byXz1JT%2FsErF4Yq3AMbMucf7DsR%2BmzFJ%2BPI%2FymjGH%2FpXsadKuXBEiR7N4%2BgB1HkRfg4lhlC5VuYH9D4AmTh3XWLX%2F3JPj9N7erRHKh5IqDkCvVJt80wM0jjwJmIDlh0cRnZLg4hIZJs%2BAysT81j%2FAm6pxYCgauOUe3%2BKtlQlGzw2oh3CLm4o40bF7ZxzSjN1ZEwHMkEkDT%2B0bvvO%2FbLaUJXyBeLOqB%2Bga6UoSl6EucrXxHEG87TG2x%2BpDYtkyukeMF35rgdxJHCdNBT6lZvbk0Vh6%2B4uBiHqw94bu7pMW34DtreeR7bLSwU51bcitq2R0HI%2FsxLF7q7SiGQDQrKvJj%2FkaH3Z0OVWIASltIT0gP2z9ec2%2FpXikGGT8CKZoA3%2B5j%2BpWaRt6EWQWk%2F0gtAK8VF5JQgResfcoe8gxLgt2p6W1Qik5yxrxa3cDqLWib6G6OY5uRxOHUGTc7G3r6rU%2BY5%2FMSXaALI1ZZcVTu2SaQyLfCwVfJ4bMY599jTohp0d7O5VuGZZ7N9Nfj3vS0PYLzb2aCctA64XVYLXrKpyBOiD59BaSG1ZgYxpqjNjG8EuuRWuyofLeDSaVTuTmBzAQ5m92IdnCywJmlGl0kjTi3JbzRz27tGJ1lFVmWXPZTaF0VEIb%2FKobD7mzDC4ZTHBjqkAYLhxssHO0SAHzFkUwu3DaohLR0A%2BDyTBmTXEnGtLBsIDjnMb47Weg998CDXurYeTRJe3JalazTltCZxGF22xpqBqdfoIcOskNpOPBA9CEE6LGowGShZu2oiGBGSPE9U8dBRi2r4PPb21u3YAEvYjbgSEMaNvRfob02JFdMqI7hbpfAeLTLhKm0URm3LRU2HJ7fJkyJxeSqjCwYe8ZcJb%2FORSZt0&X-Amz-Signature=8f6a91a7fc6923a9185c19f5f9785e3f14b066413571caeb22f8bd09246cc0a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

