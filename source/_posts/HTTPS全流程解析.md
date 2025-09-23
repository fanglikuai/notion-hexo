---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667A7UF2AN%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T050039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGCpRW79XeYReTLYK3qjdVkrnyA%2BN6wo0wllQNUf31cKAiEA9Cu%2BFegQYWjeNza928TLcNQBhK1sJE7svBmA8cTyUvkq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDHgKi9%2FZoxAmSuKDKCrcA0Dg06ngo%2FgBPv%2BktKmzCBCUA0MvT%2FRFfhE79JCatjKRt5RxOJJ%2BMsfa7sET3JjAe2Fykc3wIzW7bHlpjyKBIw9mgZjUUZ9r7zcfiN5%2BCyOdEFMzw3TiTBljRC2YFQxf2AI34fPei4bWy2wHI%2BZ%2F03A9ZS3Ud4kDxmtN9i9PIJ6IGCIRvgsYvZ73Syf2iILOdp9vE7rxeZOWNUB8QfLasQzUOjMWT3jR96G1EPXdgXIyeA8UGk2AMLDNhfZqB4o06XUARrRnaheS0D37K6XugWxzkRw%2FFbIETiAvtaA3RnTWkkjsqapKpkkiGizniuGDCvYeeRZJBC%2B%2FAfOtwGRYnJGL1Pljv91NzUXc4HVFtVNdZxadmfMDDIdnzJKz4xbtNj8cI8nx0iw6%2FhQTfxQfGmEI0xSBxHUC4%2BXYBrC0G%2FmLn5fMzpEWrR592vY7Xfq5s%2FK5C8q9D48jMUC812%2FR5VMp8lNMA%2F1kS2kd9PbomuGlOoMjlzmKzAc9LcF%2F4cEYit2m9l04YjKVb8a3FRsqVbV4vve1BsGC42sTrpLpXGpjxhTiIkZ6VBsOPsJTYAeYiyemE8xII%2Fg9V7%2BmCS66MCidYiFV0bEA4bw64RtLZl2dG2xmczHUOXBLEhk2MJvQyMYGOqUBctcyyzvnNJf79C1gaLShOM1Ls2WlrfXdpuuceKIpScxtRFZ1iW45Wj2jlE69uieFemKhQqtAEspDFY5cCo%2F76rtnRSyCR60vqc%2F1KMy9GQ1bCJX8p%2Fqlgi1rs%2F3HCQO6QRhOUAU%2FpfMtSF9WlzSQeOTeNIcbtUSXp502ugnYgWKhW5V8GV52o5Sa3B%2BBy35nfh3GfOs2d%2F%2FNWO3xzwi%2FPMHjy6mG&X-Amz-Signature=5db8664e6a95d6f8538cf0b71aa5e8b5d960b1528c3669f9981bd0dd0b0d415d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

