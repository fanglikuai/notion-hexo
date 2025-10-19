---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZLBT3H7%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIBue0SdHbQEMhyrwxp6jbma8TrRZWMIIIhuVCJdRxscBAiBhUXqM%2F1yuDL%2B0K7RdPyrTzWkL0NdOQZDwJAWSWAIIuCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMV1BiyZVS85ts7aw6KtwDS39wm5jxVzH5F7DgTnSeOrLH4ITJ0uPjiFyZkW3aXTqY0PaV72XNNF9M2FEnQ6aJpZsimg44hJG0dPODVixNSW0TOHNGIoTA7ZmaQVbCLd6GwBjYfZpJr1HxfUxoNOjpS9LPL0sP8fAFGkSI8vm6vN9Am725LIke0cjNfXkgZPjRP8omcqvpMwW4dfCFCn704pbE%2BIwbjjYinNlnbPmTD2F%2BpZpHh3DIdWcBrDp4JeBKOmZ%2FEKrWBrbJSMNCPK4RxAiml5QheEJ8ny0EmsIeLt8OSca%2FMBRYz0KjpGP3PkjeqzRnL3V9EDiA22xj5VXQNCkimq4%2BVhmj5WqJcL8HStn1I9Lz37MkVAMYVSlIdHb5Db1vczLW6t3NPqnrdtj3md70%2Bj3au5bSl9z3ch4vqV%2BGvi5Gyj3J5lSmliBeoey9MtxPVBOpSnsWxlUvLAQFKDB7%2BLVSqv%2B8E88%2BpRCAigasEdEF6y%2Fs2GDbuxIP3kHxYrmMxMjOUmL76np7hqDahAw%2F9zGd9VoyDsODH4F8UMEI5OqFa0%2BRZzDQMG17gi0iLnfUhlQe%2B8P5%2BqaH7H49144r%2Fg5zkrOHWuTc2QEfTsoNGaWzl7nquR0zNPBW2kJxS27fNCDwjJk3JNcwvbTTxwY6pgG78vcEigbxd1cNDhgafh3%2BOXK4QmvQGLFNaimqrS9wovWmzJjBk7a%2F2WPyAevuqCaTGS442lA9edfa6AMerbvlHQ8u47kkFoELw0s7hCIPVq%2B45sEKl5E8u2tZJPut2IeRWFo3MKUHhiXf8EO7%2FtLUFEkN14UuAf0x4cI9f379d1PWApJnIA7Uzp5EVxGt0v4I%2BjYxbRK8ig8xPkiAyUYHOciKGEzz&X-Amz-Signature=8d5a2787ae6b2858941a4dc8e4cf528b2d3664f8ae3ac354d8b9bc903847b757&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

