---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627ZQ3WJV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBy75OJzvjmYHL%2FHrC3SgzRUGsyF1NQB7iyRI%2BIeXQyFAiEAvA3Lxl6Fc5AFz5x0IprQa%2F8YrqDTR2AffqRNddflcvUq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDIdT89ixBqY0%2Bubk7ircAz%2BnwmLAzDlkI4F9GJymiA1F9TJpsvRMUJs0leVv85m5zGnTwUbF%2BClavVHClVv5vhcC5OY6MPFPeXyhWQJQXrIBU36swsmRx0SAz2Kfw6KVewLA3fFMvM4msX9HGHicrFc1Oxcb0ZWlwFgQuU7M%2BZs5wH2uXEQ74Ah3fX6mJQ679aGLG3AGJRuQqyVN4exh7k%2Fy%2BSPnzenTaJc6BrC5opmy%2FsAqoT1yBxkAwYnwi6NgsYnODTJqsYX%2BtZ7JB8F4BQtw5qFEc8EdG9z%2BAoz3GwAhWLuTm%2FEIhgcfsfWGQ1Ttq79S120OlFkF9VEIBAiBI6h1sizwEQA%2FN8uZknAQnKl6xQPN2tzZvwPLJdo1JhTQxkCtW%2FgRl94%2BDweuoAECNeOfr5pb%2Fr2Joqnzly%2FIetImYvrpmveSWI%2FWv50LccCXw0xZV704gUvlMFtl7Bbek1nSuU%2F%2B67Bx3ZvhWtVcHOqrWyd58ZmX921whsEKifv58woh9Gb9VARe0FGH%2FRHG3IR10NqP%2B%2BMXU8UcTE0rqciblD6WwqFaxvWrr3gCbxV2U9PbgeS8bEjLTRl%2FD2FW7iFNmmJrlFQb%2B7xi%2B8LjcQsuPfOZQK4lF%2BYj0l1rBQReRXxwTdqVppgwiYtgMISFv8cGOqUB%2BiOWNho7VgZWKlAjTXrf9Fe75lC9Te6cn2Lvo8GIvJoDaevptoqn1C%2F%2Fw%2FvG5YNJdrVdhKvPD9gyP%2BJBz%2B6ztiBii%2BxMTy8mVY5q3nITbwkEs29tsEcIa0NbzF%2FebqVjCZVh8dmfdSZQ3CYGOGVDNozImavkHhIDTLUfRtNWtxO19sB0TFo0y8ZF6c2PeaCfgJ9u8p7qFXosRsVqEs%2BDTmIAlT%2F7&X-Amz-Signature=6c54927be0db1dbaccecb2e59ada4b8237cde44fa971605a012fcf93c382cbd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

