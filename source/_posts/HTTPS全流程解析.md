---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWLDZ3VY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQC8OBK8Rl6rzrmd0nARsPgylXzkRep25U%2FBn4wOaQSDnwIhAJnpYvSEo8yE2Gv%2F3f%2Fp4pHq%2Fvr30CYQpIbY5%2FiCMAcFKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0pD%2BqSDRPqRSMW6wq3AOx0ljdgiuzzb2guxpcLLlB%2FuXi74ZrRZZ0m7o4YQwhatoXDPYd56yagcQ56v8xJasrfboAFc6kVwjYT6E%2Byw17idEac%2BGzlec81dhxOiTcZR00TUgWAN4a%2FIXvfvWnsa2F0PIsakPC5ejpFa3oOwN3WP%2BVJsI0rO8qcG0H8hyzEa8iauIvEJFrM8RoyOaoIRhEUcqyHF%2FAR4txg9UstbfJs%2BteAtGzY9yYONPsrmXw55T3i012PGPqU2TonyGTomyMfPO%2FF2sVgvB%2BV0TQ9T4jv981sq3EdJiKYBfkmS9aeC8E4ghi9CiPh0lRV2zaLkPcbacuNW2E8PIUvQSEKG9FjFcQwADnUxyOwP9cTTdwe%2Fgoanwj3gWvQERESlcxK4M9QDZeL%2Bhy11bpPIBK7DVXxqzW6eZMPlfTKQm62aa%2FE9PLfiV8F31hW8jBD7GpCEDPi%2FjTtN1B9ht%2Fw08tTsQRkudFIuMrvtlyEt8%2BnX94iZ3znr19zgGP5Imktq48XDL%2BevbY3txvbHHhutMwTf84v0FAvvqfuDhz1cnD6thOZ7uFcRe7UN3U6nmo%2FcCkGNgaaoq49uU5vr6RXEs0%2BgJ8taRkve5Av8zfUqCz3i52ZZF2DOB9zOYXW9bERDC9gcPIBjqkAQiNV8rdNF6zWYWp1XJusHnZ%2BjyXUTPSF3X02u%2BByH6pL3XnWa3sMUwUpM7gwBNnKj6yBi3SsaH9JI9bM1Z9YpVOo2DJuRy7np8Q0ZFzWupu1nrAIg4ier8w3LNRImCIo7TxzeGdjJBrphUhIQxMnJMVgNOknNvyrIHKaWfn3CjfHDUoAxKtl9C2EPkDQwhiSWggD2m3fA5abF7DcQHBsCY%2BJkBA&X-Amz-Signature=df8c492d1226c500971a328fc8c0faaf0e4ece64604ec89ced1afa84218c9a86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

