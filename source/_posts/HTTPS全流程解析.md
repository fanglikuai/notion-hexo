---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQLHMA4O%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T120119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIE79TOYD9tXCEykQVLD2qhc4QujYoq%2Bt%2B2wxEGZhubiEAiAlo5FCtm5slE0yzFeqX39JWDIPJY4CB2LhASmQeRV3qiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLIyAlJvnLn9fuZvVKtwDlCtB%2BqzbnjlaSnnsG49a47Sd4cpUqnlFj6gxUjRCyXJjNzfxKGDTlrxjkO7MWPtTxzYlUm9HQewu7WHPgsiovwZGMyk1rEymSOH2hKKTuxvtzESSJsvgAZWbu1AVB3nZvxwuC6YAWH5NNNh7yrkK2UjMBfp69NqS7agx9KjQgQyALWgRXL6pRJk8XNYwwz%2F7JCQavm%2BP3nU9yo8jumKsCA2kM10bm0bnQAM2LTHGo8kF9nk%2BgGWnAGmgaquFRgae9dHuQFYCjxtlQuQXRgdYAUttGwfX6Lnc18JxXuxcI39GaegdcCDIeLRg6eFJWA%2FB0etenlmlxm4aWPjOi%2FuNz6iNJjFotVT%2BqyfejleIeLggJHQL0T4skhgotRMo4a7a3iSR04MBPvRYUhw7eJglaKL8GKyQKOKQ%2BQ15iYnkAtS00AjO2q9EQWdNxtvNQi%2BVwt%2B1FA%2BJu2yDH6JfcljoNjGnNU6A3%2BIgdz%2FXRustiU1Oc9H025e1TjV%2B6RvyuapoHmx990T%2FGRTStRqGqC8qE2hnOMfWTzxWDhjF05TEcEyB%2FsZ9LIfOeLBs1YSty8bfOh4YiO%2BdFHX%2FEOYS4RAEXWNIJ8zemnbjA%2F81d3UxtOYPl%2BTlzFXwo39l1e0w5428yAY6pgFjse6Mvq6Bxg9wuGmhvQ8jjO6DIjs0f7vp4O1yy9Z%2F06iCPduY80%2F7C8oEOyRiJtz3AWIx5PwwhoNSX2H3r03%2F7c5bhvrXO7r0cCBeTZmXiFk5HHa%2BPwXCyN9j%2BMAi37OQBCh9MVCki1ChQycEEyyp%2Bpyq08qV4rXNkKvwHhcjPn%2BzEUIMli0%2BdjEzVarwmFs2sehoPwo2Syqy6Y4ud2f81s4nwtGs&X-Amz-Signature=753ad3235c8b9b868dd9a1261b94000fb1e8a54b0e51320affb50e67db26d7c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

