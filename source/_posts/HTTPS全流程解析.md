---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZNO7O3U%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJGMEQCIHdTaOjxas3bqBYGwRSqqAWBT7%2B0fIndVUNzVwyV5cV9AiBMqszgvWsP2k5cG6B1FDBIPqtMZIfqe2DyAPIcJ3ISBCqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDFtol7%2FmxfUh52miKtwDcjmmkucJkjf9Q42pDL3rm9OeEkW4hGKJAhtucPHEOsDQfSSmJi22EEhKceJqda960vb4l6ef3o2SYwY%2FtxzHMBQolyNYKHzVXW1tl8k1v%2F33Yg6XuZ5cHWa77M1VamitVYQP3wyuVy2f07x5yl1Qth6r1XfuuebNguSSri1XHg4b9aOadsP%2BvhFl8LzMUgKqUnC5jH9i1CB4ZSsdX8LTgv1B5b%2FbSwdrztjcD5BH7FdHGGwJGofYzwDTTacPRz4%2BWJIYBsGM1p%2BknPd%2Bxj%2BEzROQnSpieiNYYRPmrCWf9gP9PPb2%2BWT8epYHmEUDeTxqWdJytw0YEZ6zajHpa7zyuQ0pYa0lHU6IRQBBrFzmRrWHU9kM2Qn1H0tFd8fZOX1IecdHYb93SN8V7E0KsAd7ft6f8xV76s0F1ecPwV3Y0MheGCU7sRLpSlcvjCUItt1cBL63V9%2F34n0GQN5KwnTxNZeRO4N3Ql37k1%2BSsH7e9CXi1D%2Fizv%2F8CFBltkMNHOEW9Omg428vZXXZ2a2j9X5NH%2FJJVWwKBTVPYxd62fNBAsZpkInrbJBXfP%2B%2FgLB37hCVePEZy5vuwD09CezfJEx5LoINey1xbiwzwCC%2FJfOM9mJbxwEfL1WxAMvHejEw6qftxgY6pgFHdlRbrJXeibe7c5p6JYdQMUQUnYYGc%2FAVyhVDsB8Fb4EDr1QSJbAlEMAgH1NeAowod1cxe7kMPeAhxD2q4O9wAZyr9WDdPZZdhhl0AQfaY8eAl82QR6ljZtkwV2mwAmW%2BkPt9esUZS81T4Cw%2B382KqW14bOA17GnLiY3kIo5LikP6nozpW4LS9gqhBxqo%2FleJPbbdcCj5AFWjdw5IBqewOXwN1ruX&X-Amz-Signature=36f90f14ad4509f2c5c4ad60faa40c2644a8a6583530c6a7268132450f01edef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

