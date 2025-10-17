---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DNRWTDB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH8VwN644h6ht9u8r6pEkS3QCOGA5r7WUVFDRG6TlgDVAiEAqtELuQLQVbxOKUCxQf1SaNXfk1v%2F9yKoBnnJVm67Q6UqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJD5mb%2Brk2AMMPw59CrcA1KOFnvu4y9sUi%2BUXSLCIAdJblUyLbdzRgN0JvbkGyRczTXNeT4ifEh07Wbz4I0%2Bd%2F3ebA6kghM0tTcI%2FW3Im4iNY6HY20sL1TBLnEdhuHwQtG0D%2BD9gisloh3QKJJNcVCJwzRPpUAhyv%2F03Wd6q95Zc9DjKZ5vPoaBszY%2BXY74VNfZOw9undw4xghtr5SbHJnW5UESLn1Hc3Fg0GTk34n1IU3OxkG7tnJugJyZkGaspTl%2B5Nm%2Fsig2SgdZGTw0W4PmNwhcvKUMHmpFqZO3oUskHJGZvFUmfAO09AhHP9reqK1gBFf9TikrLjCsjoW%2F8%2FRrOYd46rFWuphnzYrrepNeL12WO%2BvXUwR0JxV3LvLv6gbUQGXU1JdxeM0zKy%2BXrqqOBFVbrlLjsusDI6jMQI%2Bx8o3PSqFcrsJc6goroQp7ZnMdwHMuadbNOXOYc3rgX84C951wOHg6L3Md6J3i%2FNWmhwnwiPCCj8S%2Blav8Lzs7Dq8sZt8g3BkMxtUka8oM00%2FDtYy%2Fu8vL%2Bc8IVmRnA4QO1KqBhfUAJZIDEtq4gvXy7Canf7mzNXZfdoszdaPexjPY373vywxBxrE6iR5YyN%2BnrMEJXwedVBgfzTcCdeCAykY%2B6vszwMD8nL0MDMNPBxscGOqUBQLBxKq6q9WipbdTjOKLDXRWY8i%2By6akSd3%2B6FOUiG8%2Fe1dxPpqt49flNo%2BbV%2B%2FVADunV76JSzbR2szvRkEmC%2B7pxdZRIF6lisdeOzIWe7AnTrrRlyoIEWL8guF0bbplX8iEfA4EXauyE%2Fw8xmbwdqT9lH6BWks7cxIYSgP1fy%2Bp4TApS93NiCK%2F2P4NglzqlF%2B3pLRLgtYeOIDY7cy7J1VYe3G2V&X-Amz-Signature=ba3582ce9fc3a39ca989095dd71471a89e662476832131d826143b227ad5f2fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

