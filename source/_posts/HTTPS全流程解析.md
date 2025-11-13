---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBQTHJHK%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIGv%2BiNdE66S5YIF4IKzI4NgKZe1yHOonfcDi6vSawSemAiAqQvontBG6b%2BKpxeGMlBNrucMyW4TEW26OkJenJ5S3rir%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM%2BmToYV05dHcu8Y0BKtwD8wD%2FDZtUuQpD6mPtJ3Zj9Z3Nyyl7D2OW1fjYI0bNavuUWah5Y9gtqwDpiw41ION%2F9SVLFQhm0%2BEFqdTyk5dfQK6s7Zu6svNRGPbWGA%2F3S8Gj3Y7LghII6SFuKZfyHPBN3ZGNwGLm%2Fc7tzZ9OMQOsX3X%2BwtcXttLIvw8PoPNofun5xFSbXDSHBNEwwYnIzSvbFzpuouo2jrQVv%2Fc1bPKJrvdnxsRs9Fv8lYlT%2FGkMBvcDUzUAE8gfc6xKNfaSl9s1pqlLiScHdJV5lLpkP2hPo1OEdMfbOX6wACaiqImL%2BQO21iqHZJDbBHW3pTXAwtdDUvoTvCeNtDbnMULDOoWRYtH9eft1ccTGuN%2Bi0n4pKS6LqPcoU0Na4zqUrHt9j1RbizcoM%2B3v%2FFFVBbdiAJrj9rsJNs%2BnWMUPrCCys00xBAobEldjUKEqTH9JXPsPZYBL2W9RkD8TITGEnrxR%2Bk7jvZQeBMBsIhkwYILOHPs0vZ72JgmARyEQV3kmHfRbApfjEJAUu1j0grxfUCz2N9%2FKjW0bE6H8jhrWhm%2B6cU%2By6g4CLxFxCcGrx9lHiGDG4lJ7XOsw5PqhZXIED3VvwUXH%2Fm8tN9%2BgQF838EbH5x9MscMujLM%2F72Lwd5bc%2FzkwptnUyAY6pgE9HOoiLW36asjFK%2FyHEy%2FSSFXRUc%2F7U7XJxCIS9DH42zWCCo3iy%2BZZgd13a5l%2Flm2if33ypIPuGvEov1ib2XfJL2SwH7Dv8xk%2FU7RMK14c09Iaw0K81sY5pO52N2DwZX0ttpJhIokcq0uOvWf5G8MgZq%2BfwZVy2wPodx3BKsw00llLi%2Bd0ZBxrkAD1F7hgFGmianestmqRS0ZIboxS%2By7%2BPJv%2BUbJg&X-Amz-Signature=b0b4461efd81bc448e36130645671c1ec9e28d69427cf7e77a612ac9b0132b8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

