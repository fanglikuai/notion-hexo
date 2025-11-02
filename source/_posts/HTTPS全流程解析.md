---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY7PHIA6%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICG5XT%2FMlnrxgMwXFpaz4JVsS%2FBWgtJxPM4fyNLXOFRfAiEAiYbuN2xxSU6Ej%2FoPofCq%2FOHfjxxbEVsY3Hq6H0TBHeoq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDPFBHITbV%2BHyts6M1ircA%2Bwis6T1QcR2iv2qKTRfnBjKh5MJord%2B6aNSdl62G2pzIlEKXNb4YYCnCb0gHPZ5GsOQsptcReXw1FfJboPVuMJJFuxKCcQansCAShVu5VcRT%2F8xDrQlw932nkzJICsgM2VluBa6XSH2ETqcp8kvu7Tj6kgZJX9QhD%2Fwgr2CUED8MXCwicfhalNbCst55IG%2FREPpvnerVA3l1xpK6pD20su4VUZnd2si2zPL6pjrgQZRcW0tq8Ej7olb1E%2BEOS3u5MjBUFwrbmHBrBI2utzZqN7iKCStT1yRDFa6Q3FNsMVt3cYXojLp%2F1OBaG8JFpzK%2BEYwhQ7g1Vn9UuQMWoEQKhBOIAAbet00zOMHgJIr9fW7uunRlKqOUgGayYVJ%2FmTZ7AhsZ%2BHNJLvFpo9bmEZab5SpbPBKnP2AcLndw4b9W2CAXS%2FzUeQ2PX6bSpRGhE0vTV7x5U6qUlXbDwc3Cv7sFmZN%2Fqs%2FnxJPsZee8dnxKoO1DIIGm1Q945o4nYBETaVygEiRaWnDQMRRLFrh4YraSK0CGWy6%2Bpa8665JbR1ZpwbtEX5jaaV5HG1C3Q3vC1xpuDX%2FjqZ9Ry4QSFPckl76isHhKf%2BEJi3PZkzL2D%2BPVwxwSYtHAdLV7DfTk2yeMIS0m8gGOqUBKxQGfJvHWibIlOc6s1MLOi3%2Bkv3VGlqa%2F9XhnmG4quUj6LFt28OnkPyeSe3Rzw5uXvU%2BjL36dk1DnG4XTkv%2F6HAQgYJrLj67aZ5NBoPTIrCffI32rcTA1xOk8LQ%2F3cTlpiYbZXyp2zYZSQMGptefMuUXDVWqLH9kuuWcytbJcl8uCUeGJA3ZMJXjNelbhIiXOgLtmrnf5SI%2FmsTCcmiMIdzrcAO5&X-Amz-Signature=9c917f80022550cf9cf88a29bed01d07fd08d22b9fb5c3b1d769cf6b891ee4f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

