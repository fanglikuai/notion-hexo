---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VL7QZFT%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBHZiYz%2FAfqPFxZA%2B2YFjocH4MC3NsO5RnSywO1LwCy4AiBgMXFspU5qVeUPnOn0tEDaZN2k9POMCqx2I5aRzXv7DSqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2L%2BE%2FpNYyAPQ%2FXEJKtwDKo7tklnLSsmRxsJfY5jO8TBtqH%2FRRKIXq1Sm8A%2BavYznXLKXd5iddtlf0O3P1SlRp%2F4KwMuc%2FelPHM6G7%2BvhL5o1vu1%2FE%2FfsPCTEn1Y9h%2BdJ50XwefY76mD8OZKi9xQML96eJ3aO2qsaybQw%2FLJZ0OqmUKMhXOdBeFbq%2Bnqp9E42kgNCEqWQwi%2FDgmEI8r3BfbISWAwQW%2BApJWQkpendAKdeHvGl98Jph47gbFnh%2BvwzKMUPJQF8di6aV6gd9KVOncL1V3HyG8d0h9f2RjaYo0yRpgnoigBbGQ%2F5LVoUGX04myoFWTcJy3CivdDIDYyMpe86W15zCMJ%2F8mC1fu6VHPq8RVqoG0NjcDocH3tfCXHkuB8whkmUVH1sKme0l%2FyA1Wi9rHpPVZty6UjBqLBuvy6KCVOaJszSy6qsmtxB8VLjvI3aaplLm14i6StXgSCZHF%2FXixnmBul%2FQZ8K8G4mpWCA%2BgipElkVEcYUsDN7o3akjsVEIu5PfYuZHXAGDgLAFgLovxw7chWvQzp64dlPKOu%2FG2mPz4%2BYN5mjMoC7mHHrGdR206%2FzKuLf8IWxfO4A%2BSZ00IcWsqiboTkU0xskpXVMQ9g8rKrmSN4jJQfHMUzlITOxt9ggmsdKJM8wnJ%2F4xwY6pgFDa9%2F4Ak%2BFm8hlGBffXYpNWvTASwCY6AC3Eahtr%2B4ouq5FBguFPNcGRO6Qt6kzclWlOqUILBInb%2FMySfOHaexASKy3QiXJC3xZ1sEL6lvT0GhtbRnaDKj24KeTk7k5gIJQ8UpCAnIuEtk1Pa8QYujFn%2FCKdrMtX1SQ4flrV%2BQQgtypoTOyky426n1B9Yb%2B9mUAFWk4MbtXTJy9iZGshFE4GKdEC9%2BG&X-Amz-Signature=117679512bd3494418c830ef389ea0510aa960ce602b9b3f0246e0f850f5685d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

