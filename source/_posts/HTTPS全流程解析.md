---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM7O76AP%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDUx6AZKS9z2xAnX%2Fx0AoHLYUAXvkNWhcL%2BKl1SYpwfqQIgXW9cemByUR%2BgwdZM%2Fw71bxGvImW%2BlWi0iNJg6OLcGmAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJa5VLpKuFLvp7rWPCrcA0kY8aK6Fg3TXJpmt8GX1C0%2Fuu5kdRvm%2BUB5UZXARijQVsNyRVxsKokY01kVsJ9KSrRMUXTDaWaXN3JCGvc%2Bs%2BpyZfPpjPS63t1xo8uOxjj4nRVIuU6M55xUMo4Fh5VFLA8kJ2bWHX5fMyjXbV2oGfvS52pJEMf9klKANVsUAUO65TjnE6nuElOCTRznzqB3F4NAlyEPerEDOVbRqPZmxl%2BX7LqScrJNsLk3c9VJohaKnx9JSKW1XfYHU%2F804%2FZR8cHF98Dfr50kfCwKW3jv1nUimLVy%2F3UU5UYhggVH2qYg7uatIbUegMdgok9Riq%2B%2B6VpZ252aAhtYxdAiYOJuhEejXTEdMbcovnW%2F0IV5Odqm0345UqY0lU23iUfmDcOpTseH8Tnr%2BWCk17Ibab6k9t8HGuxC3HP9wpKkdci%2BTAdrVk1dSus0vt%2BHnvP5P7BobN58nHy3z0EgBE0dv8niJQ3Cg1DbVirtFhzeHftdp3IKtJ7upp5lphqvdgAeXD3B5x2dm1nzHxeN8SHX%2FITXu4%2FA9uAvHQ9XYxJLylKsqSQPhF5vQv3D534gOFInfDpmwN4hzT1yFx9L00FVE7VTh%2BJZIqnK5qJWXo%2FzB6deNjSp7%2FniSO1qYO3xbo9OMIfhy8cGOqUBpbWBABgey0OuQLVj3LixrC1LHbzF4oKpe8oAmV7Wbcn1Ct2n%2BmXAjNcPCOfnDNlZOPUjHf7UQnsfqui%2FC2Y%2BT3Y9SvLeja376n0KBwsXvKi0%2BykwcxI%2BSMBRuc43aevl8XQqxtvcqWSjQ2wZ9Mh8wPTeVLasqk3YWkYG%2FnMoHWAEgl6Ue1gSfw%2BKHiRpneowEPB%2B88ehUNdr09f4dtaDeEJDF7wA&X-Amz-Signature=cf6f50b60085484f57e27f9af3f263203388805cbacf73b5c7ca3025b8202869&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

