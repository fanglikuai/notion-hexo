---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRMV3DBK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFVXkWQ4c536AA9UCnFDJL0N60kmHHYtVSJC8cGoW7yoAiEArQSSHQ%2FjWJTEUFasMroeIFarNZBXvQyS%2FMxrKLwRV%2BoqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMrSqhDcBa%2BSGRc1YCrcA3LFonk4LrWU0ZEC8kobU6AOosoqCPTzKxlxWEeHgz%2FMHjI%2FjVmoPN0eApBhff55wdMGClww9mDE6T%2BG9lxvG4J1I5qZeXPrRJnykdZFIjKkyVscnif7Szxi%2FAXtpz%2FZEngi2rm4JRPbAL%2Bl4EJzR284dfrkXvxJoh%2FRk9i3puK3M1HLtVT3i7h%2F9a8UD4r9C3By3tZfqoVXIV8Qab3RrEr89DwNKrbvexYoehcCW0Te9YACylfsxE4k9vNPQhula7ZllFGktOw1CfoZqZm9tgfR664Xs1PvDBIhy%2B81XiV1iIBSJ2YKhzqAPr7hDDySfIH9AvyBV9gha46bs%2B8K4Matr7m79ogWVbHJ6cCJYLtu43uLSCf4%2F6c%2FLBVUf0JW7GOzlm8q9q36%2BQLy24eAXkv0iY46AFp%2BKf3x%2FcSK5e4Z7bV9LA1U6gR9TVm6mMDfwGf38lBMFL3TqpRAnpT%2ByOSIfYckaxn%2FdtlwNCAt7ZEe5x71NYCun2fU7zeCRSTtOjyItimxUmxjkkmRNz8OSogWb5FKp3MHxKISMGAY6cnxAbSXAtraNCXIj2eKZFB1sUHHe5xVzkWqIqQakWoPCiFs%2FUt7xRvBdGgd%2BxYzv0f1z1zmMtyOXrwiGr4QMJzF8cgGOqUBiz1Vfl%2FsKStKwjbj9H%2B%2FV0UO4STJtpDv%2BgPHNpC2F8pZGWlE%2BlxV7YoYyCdTH8fUti21rnx2M1dMLcz8mBmxpR1CJfYgoQYO2M1fR3sYMia18WZ%2BrMBbfePtshyC92StUpNXvYJv0o2FcvamL0qCi0WiAdUPNjq22JfduGH3W14XWyCRInsdj9AzhU2h8rxgq7ZJK0q1jCxCNrrf%2BXntuEs5mkoP&X-Amz-Signature=5a35ef171c97e8e3739a38fb8d3c7a0989681fc11ae0e6f6dbd17cf697f8773b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

