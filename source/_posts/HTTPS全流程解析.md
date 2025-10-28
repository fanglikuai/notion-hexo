---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOA6Q25O%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEOVj1X%2FlsOeDS%2FklBd9JPDmjF%2Fuuw%2BDOhUAbEqVUv5pAiEAjI4ILFoPd9r8eO2uLbB4JBNDVfkEnpjQCuEaH4MEv1gqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFopVwTYsjYv5nU78CrcA4zytWFEs%2FNM8F4hkx%2BnnH6QqzOes8dZRwJRU7WQuQZWDK1CPCD4avHOPbMOJ3zlunjOoicav8UXCjguPs89TBEbt0wVuxgOmYygCit9U9rwrLEM7kB1Fnd90C%2BPyd%2Bsmum2HUJ%2BDErb4kiqvtolRNjfMKkWPd2LwdYOrc4Zu9AyrOs8MU5GNxJl4EyhNs8APOgGflGR6HcUEauUqdEFv%2FAJZiFL6fFSUEzIuQVhpdtlChgSB3W3RkxSdS9aLOPRG3ZXteqX2%2BKXg4JUqEtolNOXMgEnVEzcLn1k%2F9AG46NtN2LcYy6ics8GVTyy%2Fv74LDLbHfCaHt7nMh%2FPJQ%2FgeQp9PL4LPi%2FjI6pE2nwVfUxTC6A5RkXI0asLkyvf%2FlNH4bRscNZSIAtuY236xuWHcOQCVuoatktOKfZVpBeX5xx2lSD%2FwIPokOo189ONEsVywG81HQMMZxZYz1toORyyAiG5HU5WzzLdYOX16hPF826zH6YLDsvsPFvs%2BY9h3hajmGiaytF32kKutLHD55EN2BAMOdRD1VHwQ6HU2aSQJ74Cati%2FfNjn6vF6eLKap%2FweohoGVE22vYxlfRs5RcWzQ9%2FuweSPgirfcMUvMsFqmw3GUZwI6psbNMCYwE0wMJC9gMgGOqUBiZYFhZ3wGXiCkhRnZzE1GVzESguhcPOVDZB47AW37wZ%2BwvMpxuAM5DD3DDDhEUGKrwnF%2FCxB5q3w5cQPpPl4I4p2vCJ97dWixmBdNq6u3DzOy8h4ozBSfaUNWdeq0yCyN4WbB5FJdeEWb%2Bhsj7gfIZjQByfwRNEibFVs2Cl1PQFLJjg3csccTa16X7yQcKf%2F18fpqTZiRQ9c%2BTg6%2B2Lsk%2BC457Ko&X-Amz-Signature=b5c9590291e010a70732eee59b7be7bcc66d279bd99f4d6a038ed49c8632ad74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

