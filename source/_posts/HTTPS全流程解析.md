---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U4GCPUB%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaOQmvEyFFgeX2U7xLDsOqgBWRLdL9xEFXbNYUxOnVsgIgdLktrfJNk1hakTDuI%2F1ruj8v3cdOZ4dcIb7wd6aj4Hkq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDCl9o%2BUQl%2BXRVy3pKSrcAwLE%2Fw%2BU61jTQ61EPSPpj%2BUy4tbAc%2B4Mfnbc7DewQqWg9b08jYTk99xki4Slx1mQ1ojX2TPoILuosezGXNCqZVAVcvxuheTjWAoHm3yB4GjVk1EJL1OnH028TDBpjIxyQaGvoQriGQXiZz2vicZRW6OD6pKQEKU3SHBIF%2B4AC8Jov8mM96alV4eg4SkAFygtcwlzliL1RCw8ktBcZK6cwUu4LZPORi9f9PSEr0lMkP00LMCt25qExaE71F%2FfqTc87vLVNYj4hslqQ6OKPS1uy6tDQf6mSwkYMKR8mMroV6x%2BoJdYzZ01kORFRY33rrnjo%2FTXcyMCjq94cOPEQFLUXnAGKvGkwSu0Ax9ImSlA80dc9FMDGggM%2F6%2BSBCvO9%2Fcv8y%2FvOryfKHOlRVDQofW6HGj9%2FN4o737FHvDXSfzhTdSieUQdiWmrLjcDBGm%2B9z%2BywDR%2FiGoO19r1x2vrDcFZG3%2FCLK99HgDdDhgkjRb2t5H6mI9cWJkLgjDcNbANi%2BnCqXU78DIEPdK9NIb9WpqM9JeajTxW8hTMZl4%2FspjCp5g%2FR78aSLXTKHWYU9TD6k5eaCXgknEMmIln0mzUTNzFxJlOMA%2BEg4LEH3UCA%2B3m76N9RKITARSpx8WzDYq8MLfq8ccGOqUBlGVvXhC2%2FgOxwJFZkKXMVM2osj1943HiTC%2BJ29DVJkIsGAUZhA1Fz9TRSGuhXuwhXDUAvI840aqfMaOEyi1AQu%2F8ymvKzX7%2BrwMGQKpR%2BvapCYl8aSeKzDjd1cIb5sKCUCf8iQ3lbmHXjim5FQwDoo7XFCoy%2FQdC2YZLUHKwy0K1NBenH960Eejm57Ux6ZPA5gd%2FfftDnklLDBLtMLgNUtguTA4D&X-Amz-Signature=1df7e84883d086553b6d9eb89bad5792ef709c63892e75b6536b65f94fab7710&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

