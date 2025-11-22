---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLQDJLI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIDuj2KcDW4U3G9%2FBZ%2BhlVYJ3aPgSejf1VFpp1H1slEarAiEA3A%2B91E%2BPM%2Bl614iCKRopdmr%2Bhj3r5JAde%2BVajMXpDZMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDAV3%2F37px%2Fyx6qnpvCrcA6zINM1%2F8Nl9wvtDRR%2BNeJ8WdwMA8io9B5mjIVqIa4cD6AeR5YjOxKluac3y9XuYYlYhz9Roh%2FfTW0ocWkV1xnx1aMBCVriuPsARjZUgk9EnEd7jSuwUkQSjKsaWGADy4qo%2FNMsXtzFiKErcjoevPi%2BDhqyVsoT2Wl6DN1nCWGf5Up%2FfI6uEvGJaZB3kP3Qg7Zyz7aIIUq1ICW9hBnRPKT3ibXEWOyfwAwCE6tIEOx0W5STKF5NPjYGUWPN6NWDCjfGhAtoRAwAYTcq0%2BMWmR3sFMZKsY4xWFL5GXJMnuyZsRq9bflpfJA2u7x0rDn3%2FQgq9gZZ%2BWrIlaU8waOhSCqAvBLeM0h5H%2Ff39urIzr%2FqDJYkVApFMVlI6wO96Gol98fHMqyFvx5oGGis%2F4FhaEhN9ccQaQBf8xC3dYlPUuMHQ2vS0twYVPQISEDCherKkBJNme1ZDqZWqT4%2FxkroZGbmejqwS2g1yE0kCvuybNH3P2NNjHXl8GUrZKlRKc9BhsPQXvU93b4HA0xC%2BHXDPlgkRzCPAPfHf9WCILVhUmns4P1IOrfa8PCrpc22FgQlfv8%2FKS7EQDF7tNq%2BVzPseWaDGuTtwwD99zjUvsYjKy62YEDLZgLo1jKCpRk%2BsMP%2FFiMkGOqUBrtwrht0Jp8Je3hB2qiDYhusStsi2UAJ7VZV2lmSTeb1e%2BVMBOG62I3DBSTrRqXwgdZH%2FBQx6bIehluisBRy90blCTu9k62%2FoUOijRqq0AsyobwizhSilhG%2FS%2FSUJC%2B6f5BIbmoF3aH3gMT2jBtiQfvbZoKNlevS0CsHhofmsLasOA9Wr61ERhJcufL0I1sR7MXfKlNJW1157CJ8xGjy%2FaMWiQaxC&X-Amz-Signature=2ac7f77794fef4f972e2659d8ed1d7cb16890ea7bda4c526e25731a668f9aead&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

