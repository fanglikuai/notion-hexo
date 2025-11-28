---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RIM23B7Q%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEx9lC4915yce5%2Fx5Abt2nRcSqFZji1QqYb0pky40zexAiAjiNGq6Rm2Je648d2OUgu%2F32AEczZKmY%2Flr9K642ScVSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FJMWteYToHr8voCyKtwDtQDLVCZt8RyXZECfXF1louGHO8wBeeBJWpXZHIKSnZFV7e%2FPEzeTaGTXPViFcas98dUeKiuHNGRmtH50qE3b%2FD985rN8V93tDVSUAPDiVrgBamcnuMTkbjDdqTLvbJ2bl2HRUXjS6Ae7DJoFLH25n%2FH5tGOxEfen4ij1pfPe3CKEYb4fJYRJ5peL2EwcnKOj56PdV0eXRHIqrHLfNt35znRqS3BE3mEfa%2BdFgYBKsCLo3Jrr9W569BZFf7XpGKnWwzuEkpIPgl3NIGbKs0Iv7AI8eiewJbpFhMvp0yCnVUNb7RR%2FNdQxx5SeSLFYgotgyiH3aLBhYH9ILBlQv5OeJOWh4RPA4FaCMyTDrBz%2FZoQAt1YV9RqmzeFJQ67ATdxz4QhWRRSnabyU3N8RmQb%2B3pLoAHHJAwAeAljMj9SK44bU6IspH08XJ7LZ0i%2F9BJQyQN9dSG62s1C5b4n9Ouu9%2FbkDI%2B20Gm%2F7kmN0xUH1N%2FUac3GEWB9sAZXwiN1pxo1GR47RlHsAPU5I9NLZp5QQF514C75hpOeTYVUfmV2WIFtwC87BD9WKtiefNhHq0TM3VsfbQvXwgJhLEm3V7ACmm%2BGpWmMKSNGgAyan4dEtk5fAzaxz3N1%2FWmF1k9cwmt6jyQY6pgHVUV6Dvo%2BG8x0tnxoxLnATPNS2H8NSUeLn5jwde%2BkYGnpVa8NG%2FauehDj3aWIs28uxO10C05CtoNHoeZpd44eiwGtsBTBKx5uy3Wb%2BtS8OtX72%2BB60TLRtVWrng3x382UU8utUbcIMSk1UkVWdfleJLACljI82MnALIISaqYFpFrFWGEpYVro%2BU%2Fkrm0xr4E1tpsIUhSUsd9aM5WkxYMPHVQY0O4%2BX&X-Amz-Signature=9ea42ef5c91a97f6f48348c87825fd1a3018b61c06ae68183e88bb108aa00aad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

