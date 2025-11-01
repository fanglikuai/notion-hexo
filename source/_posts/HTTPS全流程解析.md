---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665WR2PYPL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCaG6rhKalIeR5D05uXLzgwu1pb9xgXZYIEHYktzWpy8QIgDBW6UTwXL3xFc00b7b7xtyQrRxzPC%2F5vBrCv6v3IbDwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGu8SdlTL%2BX8DDeanSrcA8YRm6CRGr18BQgF%2FlkFgwxT3ImXCUSQ1bCgwyA7w1OzlM7Xcx5LA9EEprdee5yKxweju0BMnNKenvsWAx8wzVofm%2BmR2DP4H0NpTw0bJnqZNENz5z8U%2BrfCv%2BPRZsnAag2ZqG1xCa4Pw3BpE3ecEIajPzWpCvLI8M97DwlvDZgxmGm2%2F4%2Bb1n3qUfuMPEUjgg9K2e8jbP5WL%2FPquaT1A7ixjWBK0M1PF67b9DD4ugaXe6LzOpzHHIXO6NvY4LfnGFNXt146ahwB73F7%2FVByD5cHONmsihbTkBs%2BGgMOUmLqikW7MngxzgbcM%2BzGUvzoDvy9CBfK4i0BZkGtRzQfjwKK0KlV8Ag4ugaWJF%2Bs6H%2FkjTRsS5VyXY9tTxXmhwhXSI9JFrdht6cd7btqcFwLjFWjHRuCnfLe4M5vJjczjOsLg970N3jXiZw%2Ba6DZ3U1VqkaTIK3BrQM9TeDakoRXyCSqusn1mesqj1A1aqdOBcYZtGN%2FZKA%2Fs8NNn5V6KV2xgvq3W51cHfuQmA2uqRUTeZsaw9BOnhaf2gl91fisrEPKHP1t7rR%2BHYrEMj2yl8q%2F8aaFppMcxKWSXUivsvXlQRaMvLMEhw7VIwcmscabfhmIDZ3G7UxIbHjO3ULJMNXhlMgGOqUBc41NwUyCSVh6W%2BBc%2BnmbXYDtvxLWjxKXD6nukm1UgPwzdYGeuWcVlV0fSgqHR%2BbTUwo9HucTdMd7VvI6wr58ngXEHRnIYc29PdZcxCU8YDXIQ1D1Sus5hs46wRuFW6ptKpwmseiY3JNlpPPJcNLSIlwBOVaxO9DMFZ7Wf5hI6A7Zb8g0i6u6TslSjUSq%2Bg%2F4xK9vVqA0drrwUXfvGVrq%2FshQRbjD&X-Amz-Signature=045fbf1c61c0712aea7d8ee4f0be38e5f7f4e3a0950c6cee5b20bb778b723724&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

