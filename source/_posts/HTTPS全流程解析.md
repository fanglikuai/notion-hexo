---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SR6GZUZO%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGUHPq1KuSF0qCmRNrFTSiezhK2D458NfeBORHr1bLRVAiEA8jXFxmuIlh%2BqCzM3piLCBpJtjkzNcUxMhX68Rn6Pjg8qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6HUabCEcuey4sSDSrcAzJrZt0AN2cESrqAHhJzK3x4UMOqumv8iTcS6T0kdK6XtX95msrJvsThiwhLVW%2FR5e49PBp6okx7IIkKDBDIBKPtJg2ttWfOmvgxQ4pdyYd0Clr48gfng1gZxxRqPEbEapnkpiml1X3ZnWtHdJ4sJgoTGZxTrK4ZK8zP8o%2FnLWizMWc1Vs79IBanu5zZTYXWcxDE1xd721BL2Une%2Bi1fTivEBTWecQ9CSXvw1qwl2WmW5eraveyg00iFnn77uIPmXKakTaXwnrImyS3ZHQbxF5EwKL9IqZZ62rf3cSXt1vzMhcAVhYBNULVQfAylCJDgZx7wccb%2F7E4%2BoQbI2%2FnTU4tIbP1w%2FSX6h8MvZ5mv2YU6LS27gs7cTaOiwrSBjieIWt1xh9ga2OXqACr4y%2Fn3vh7ez9uqW8NBjR5VXa4K%2BVP5MkAf88PU%2BSrtSfj0leTCUxW5l3WC%2F3Ti%2Fpp6%2BO5v9JLcmlTY4hpEYEpjndxEnEChnr%2B%2FO4jEsvw5%2Ftd8w3WHV1GwR%2FiMO3CnbxTV57%2BZu0IrpxoXAjIdRd61YHelGtCaiPccRWVKwmq7P6sBRfW45GXBV6iDi7qHCIx2y%2FULhkMgpuAWrR62rztwSNQl13kixDVYq0w3h%2B%2Bf4FqdMLPc%2FccGOqUB9Suyjz0MZjoy6%2BXEXyFyyH6DkRs%2FkvwfJutvJaf%2BtkdHaigO48Pa%2F3uTb57fLMXJvSihs9bYZBEpcTUMwKznlNj6eTR0ca1sB57aX6Ba%2FYnHO0wUD%2Ftj4oo0McEsNponnc%2FsH85kZ4KCtbjI2ybuYC7iS%2BJKL3kMwXsh2CTtJ7RCJ7McH2S6tEujJWHB018s1zdt5YKk2nKvpN5uZUzXjgMjyRF3&X-Amz-Signature=84e4c2e689bff38638593af701be71519f06e35dc1c039b12d850cbd706da562&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

