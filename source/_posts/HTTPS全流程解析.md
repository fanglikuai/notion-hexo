---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654H6Q6GE%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFO8MtAngPlfiyGRsKEkMoh1snkVBrK1J95LP0l3DN06AiBltyqVfLrK80kdabmq5gJJ9iVDw8qj80Ca293P4JUurCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMniX1S4MXzxLai72HKtwDfho79FP07Kf%2BZrlNAJjYz35hBQvOJoT2%2FjShQq7DQ6%2F5U4yn1KI8Hh6EWVlL3DwSvahAuNGPu4%2BW0dAJWGmofQ0fU8vtxQTtvECsu%2BiQAXfUFsK6B5Iy3fBnEUBeDh%2FMYgt2P98nP%2BqHffPyjCwi%2FL0e%2B4iaUddum4yzU3pAA4AL3IetaNoOZ4LOAnDhJEwW%2FP8zyw8uyxG%2B7H7XQ2nL2mThr8QPNT9lSUw4QtiTqPV7yOU7c7VaE3daVsLGoCdr57xzXzLBCxlYdLkj0czUmp1EldSNII4KyogVqVebbaOQ9NpQOtAUR%2FEmDZFaOFIJNwWHly4DhXCM98zEby0LupQZ%2F5DIG4FXJi%2BkTlu36qsk0RCEzRtElG6ML7GiQ4JJgGNrE%2FBLbiCPbcMTcnCiecY2ebX3jTqZrPOYXtEklH0yX5mzHPgtFjh%2BnUjzg6t2rFL0TDrIvvx8kBjr46CLtu%2FXclpdf%2BOQ24kg9VxW%2B7D5EC1iQUu5oABrhgOgpYg1FvoN3iLxsUKetrjrYgA4Pb3twy5Zi3bDiBFy7l5z%2BSSBw6LwnZN%2BET07tBXlqPDPc%2BbWoAj56y2PyvNIy8qwBxyUkrW5q%2BOp4a5cQy6B0AGwUA66%2FicKA00juRow7quPyQY6pgGyJaBt6F8PleaoBBfJ%2FpfCm9%2B8kvmiwEbiYq%2BksTD3d4jmEx%2F9FhTDByXKKfgoORDJE1xuYeNccMsZGMLQD4MvHMfa9wC%2FievmQlMnCQfHdEhKYTWYKBcw2Y71yNqmsoHinj19porfaJ%2BbkDMqx10xk8Dg5zxJvAIrC0I%2BIdn9U3kR%2BhRw886YCXulIvoNK33synd6CH6nGGqiejMxvw4Z5FPDwlH7&X-Amz-Signature=a98276d1bc89858ae93121f744c8ce5f0b4b8e8750820dc1bf5c52127211c2df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

