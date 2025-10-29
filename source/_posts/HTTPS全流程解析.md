---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DP3UDH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHv1SehUpn%2FcGskqQbyKHqK1pELvH3lCzgBijIoKA4qSAiA4OBfhcoZewfOe2r2Fi%2Bc1pwG4sliiV8dM5h9yIleilSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXxhlgCO6kKJO0tx5KtwDom1TPOhXQarc4b1hRMlyrIjgAcAmg5a%2BqF8FI2eCO9Tz%2FR%2FqlTof4CTda75J8uq%2Bu58Z60lRcPyM5k6p3jMhCWswf38XV3vNvlH%2Bx%2BqfqNwfBVPbkJ7fhW2iwawCM0Hvi4QuOrFTbPsq4scEoROkYLTNLDbUyc2mQoR%2BTHNeQxL519D1bYO6LbB20hAS27x0Cf8BBLYsAZ4ViXa%2FITX4r0gGmu6pSiVc06rx34bQe9PAmac5ks2zDAw%2FhkqsVBcpn9VKmToJgRrodPQqT6cQJyHRv66hoKKRQb6I8JB%2BpNCO14I57VnA%2FeOpWevyQfuPvTPOX3tQjH9egG64QjjoVIvc1MQDCb2yLTVgK2L%2B%2FU3b3hoXabKA8lpDd0pVuWMI%2FLGQx38IiK%2BeC5gl7gb7b48Lxp0DQPGiM5dQeOsAySGKa5dq6Zdkmk6lP9DylJCvjrUOj%2FMANp50UldxWrIBqoZhlVB%2FmrEBScPwemufDQpG7ZdLLzFUjTDFjFGanJXgXLHmKE4ZWlF3ZdKdT1Vw8e9x40fwfcIyqhZpYgLenJFlxid4krMrZivrzRbtABaItel%2B0Ri%2F7CGNB%2F0Cf%2B8hEMuOK48tbtoJHsErVQhHZHrm6OtxZn9QUnAUwwsw5JuJyAY6pgFyaOf2H9dHhrAdcZaX%2FA6YfjHkYtCcd2ZmTJ8KoE4IYxy1mtPlfnEWT1A5HlNTK5qlEGJvnW7%2F93GvcLIz59CMlk7Z7AB9xgZH9qVnvF9Ea55lRtvyVleOc%2Fm3yP5tGttUCK%2BP4M1B0MII5Vp3HQQirc5wVhAwGJgmC5EjhUtIz%2F9GbpJ5WB%2BRpa4RHntOrlOB3ikQQQgxXztPMeMYbXBnh68a4Esr&X-Amz-Signature=72f94f1d5d4edab923bd2d150a5f2e65672f68e134163106c7e2094f8d45d1e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

