---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFYF6K2J%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFzpL6anOSDEQPA6iRZtaMQYrRfL5DZ30X%2B6PfB4CxgwAiAnTlmAXYJ%2BGVWNpYrFBV7tIQfbWhMFzgkn18ubKZ0y%2BCr%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIM3N3Dy6wkB0%2BfQ8VWKtwDSIrIY1Ms59MvvbK3vGj4ogbrGP1lpAz7CJ%2Buz%2FCNz4e7aIVv0HrCmmQ6kBwx5%2BPPh4nuXmX0LzX8ln8AuI4Woy9sF%2B1ootG%2BK0hTuDp6F3hS6RDJKgq0t%2B93n0u8vyGuMlcmC5sB5rU7EYhqFNrB6ll6dvrjdk5XbVEsGwXtquRKC8N9Rv5noyTwEpOHQ08OtATh%2BxrurfIh0zbg3zQkgmitzRlFCKQTZAShT40ijl8wgPPquy7feZgWxLq2cWGVqoUrBJoLT5o3naqG0Ur%2BI9EifI8MZcYCCdM6GmX2JrYOOAUdD8cIcNTHOsQOtNzUZectGTcpr%2F3chI5rdtOi9Xqi5wQUCkKsABAx8iyWqBbx%2Fiobe9mZd8rtOdciUED8buGa1YGO8KrZRkEmbIpMU05OyOWjfkJzoK3VjPUAy94DnmzPPHnuxEpT4u95cfRR2KUEd1wAoAZVvfFaDtFg7Hgd90ak5AvD3SCdD1pV%2FwDGGiTRge5V1S%2FchtSi6ecfiR2a8mlU1GOv0qIcIobOGJnYIt2x7CIFe63ApqEsZIVMKGusNoPaxNBrvFvnlfHrmH7vK1jqRTNHS6ipnmUvyprWJX%2Bn4wlejxs6Wc60wIvSyuxn986ahBM%2BszMwjdqVyQY6pgGt1K5W1ANko2lKRY9XQjeskf8T%2F3H8kTbA1j7nhl5MwzOBLlNtDZBIeyymoMJKYIc2kXtKY5Q2p2Cz1ukop1RD%2FGhr3440Ef%2FxMjL8ghuWkIssZs8NKEP2oPSCpb65Fxl0wiPXolcYa0rg9KTBZsVs4n200ayQ%2FLAw3EIO1fsL3Fa2gJAjqtEdHkvdaiLWvhriVJdU88%2BiucpFJLzBzfosci9uy7OY&X-Amz-Signature=85fe8f064d5fc23f511aa5517d33af252e1a6fe36b1e8cb6cdf6c4ed9f0b5043&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

