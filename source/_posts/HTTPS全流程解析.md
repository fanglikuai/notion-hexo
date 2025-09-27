---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664BG5F2A2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICfv4O4irpvDWi57qgsQArffDAgeD2w8SFYDSVuu%2BsN4AiAQjWGCw7ww1SjeuHnDobL7gR0%2B4m4ShzBCf3p67SoHryqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTDE813ZHW%2B8hrxuaKtwD%2Bhj28kieJSP2jN%2BcZqmoahdy4zEsK13npUW3pHKK8MHByHqkk96156h5brX6aIN3rRDsqY7CletG4MnnsuR%2BATkLPV3r90y8Jghzh3I1it2nxdXr1jpUujpOsjg4HtRv2E2EPn9KVPtmV1zLNNlM1I03hq2Gf1bLHNUoief6QCrszlv1dTUeE7zv8HnPC8Gqonrhig%2BqpJq4oXX2z5qL9dNIOj98YnrU07bFeTpscU%2BlFN%2FREKSZ8MrnXAN2E1RzO%2BhqDOV0c7uIa%2BpFHMTkx0uZUl%2Bt5pbq3iwF9E%2Bp0Hr54yryO4Go2gZY%2FQpjWg%2FWA0%2FZYWstyVE6lcotIQS%2FWjBJtKC%2FkXftHnX5QbYcK0Vqf7T1FSZIvJgRQkRNoq7NjP7bP4wKZTuFZ9gHMN%2FY8kqH5vUJosSzUf8MLGUMfTnX84wIkSo8y%2FBnRqNSSK4GUWT%2BbW3a6%2FrekZOTlNgz6v9Js0AzEcnNrLlHEyb5W1A1CltUKfBykA1PMV7o676yZ%2FgFK32X%2BPRmSP5olUy8Pyw7sBZ4fErgNEdGBmkcujepO2oo2h0Qg5FjDQ%2BSFzqbADtVj2izM%2F%2B%2BZxE5ju9i16jvDjposamZDfDrLx6iL6caMQIwclo6su01jgww88jgxgY6pgH04B5tAr60n6p53I%2F8fN%2BxoIIhhcG3nmEq4NtXsGACz15Wazkca3b88Ccbrcce4Ubbpcv7ByQcF%2F5giImSxUakIG0WoGu6FqrShxGsqweM2EOf8nf%2BZpc5Rm5erjYRaaH1IXECN0gGnd8i%2FWigSpIT86MO1w0BXmFIPFKGNLjdTHi6%2BE1O6ttn7fCtSO2b%2FkAtqyqSloTIsPU2H12RzaHzRg9K6tuv&X-Amz-Signature=0f0fc90fe2b23e51e281c99c0c1f5cc58cf62d2bd9077e1fccb2f9f249125817&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

