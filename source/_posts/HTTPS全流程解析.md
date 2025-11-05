---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDD2LZBO%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCL2NEao7jLZigIq78ypr%2BHdMC3nwcsVASunjoEflaqQQIhAN1pRAd8jlCdm%2BVN2%2FpqLyAjSrA25gsR%2FeHHcP4p9hhsKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9wnnteIA4bh6S3bcq3ANwnYA7dDjX2UKNTte2s0Q6l%2FLPgym4az9i2AdRipnNWs5M9607TW5cQq1YuWA%2BJ6ZxifM%2F47MfIT0F4yMLcM2WbEYbhUaSGtxsJXBU3GK35bPhRFDuJwVY8iy5466tHxOUjkFbYOX%2BQwp5bFvEJyguxYTMqsXtGds8O1AJMOhzVIAPSi52cngt6oNnJGrF8FG2o5%2BjqI47GNvAK0hQG4MdHZ3Mp2XWDoHDvmYTz0huY4JnEsR%2BesrrEFYuBxBvLg%2BV4qtMK2VzrncP8NFU8HdMvuafP47kxzh%2Fi2L9tPIjTaH%2B7ohbs54VOJ14HRdmePbu%2B6enIp%2F2UjPTc1ZnJuk7NydOM6VZbZlg29WqGFPpL%2FMVNNEDXYmzvohmZrp7GxK0PB66oUCIzeaCfM8Y5WS766qAy7%2Fm8aUqBygjV2F29s2Ey7M9GXVDXpiiPSg1IJU5Iffw5bboVDqLIGIy%2FSiLOlUS%2FFq3we2Hx783SUY1QYhxlVyd4T1Whuq3qJIKV01gj8%2F7OIbqgHuxppLq8kKrehXPPM1OKYGu4bP2iGd1nB%2Bwp2ehhjbnXTatzTKT3e8TwvLe%2Bc4of4nEJTSHkUprOH7LMX0uZl7gv6hMvB25Xegz9HyAmhV4ny%2Bj9TDb46vIBjqkAe5UyzLwSMB15tmg1Uodb27Q52ueNshpTgUTTqZiLAG0iufCSFURwzdJEkKRArg6F3P0FfcU%2FGmsSFC6O9YOsrSFqnl0a6C5MwFT5eXL8uFYiyPrlUE6bZdaDCfbSLXXaL1RpRuP65w6t9Xq%2FAh0GVNVrFpuDhxVmiODqssUXbf%2BmPuv12zLW3IvnNxinedmZ%2FFhFRmtmKCOItwvaPBvf%2BVqBTRt&X-Amz-Signature=5f8f2ba10c68d727f044fbb8e08411983b9193a42c66e0b8d43884a8a9fec38d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

