---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAE4FH2A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFOe4RIEDA1DAOI5RdFiHjoOTdOY2iyZEqYhGMg4jZghAiBQrCgfd%2BOGLT5PZCCl0ZhOfG67vCJOuMQE2epnHT8R5SqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJ1H7n%2F65QDNeAA06KtwDf5f96yCu6LnRAqjCq2SfORvD2huqSRxX4OQkzzv0iYs28%2BPxP5UhLIFyfA%2FOW5Y9NQXfEa%2B%2B00I2xu4fBVHz4AHT%2BWSwH9Lu3mUTuMAyic4DiGVEyEDC59Bc9p6g2wFSi7Xmz1SsRqrT8jqJ95wCVF7KH%2BBivSm8PuIwb%2BbdzpoZrD4TK%2F7InGA3WsX8ZimEaJP24hIGVjTgofaHVa89ltYwVepi52kDahX5Iz%2FawXshovXWvaai8yCWNbeXwFGuYXZOvB%2FaQefY6akH14eTGElc%2F3kS4Ci9dTdUdrLh1A89WPhxECR6iqNYm%2FaNYbubLSI9IR05vCfqSkSYvK37OsL1%2BCypYIT%2Bc%2Bqece1ns3Md9P4D066ggeevu8pEEbaFXxfn%2BsWd7aG5xsD6FMZZ4G9MzK4lcCrcPdGlrAtYCpPL1pRbXc3GOC%2FB86ogvUay%2BYP6fnrDBubRnNCwgv1HdEuksBQGyrgxyXNKFTawlmDImTm250WwtRuS3gnGIuynvREt%2BKgypStNu%2FhHOM4JLAN08sL4IgXBkDwWpVf8%2BbKVkGbtyrXkMeYn9d5eklhBTUeBE4ZJz1agUsORNX%2BHKGPnq57ipoyLiPsbp1anyRj28OHoTi4jhCyUdE0wnZ7BxwY6pgFvlya5N3xoKjiKRBMignRze8NX1RJZwbSUH%2BpZA0Cm4VWP2DXgvZbLgewkRn2Y78SKUAZEsrmuiWqmNnztpZQlt%2BVi2BErPefLwhz9%2Fve69E1XBNIwaWAhR9MKkwkSO3sS45O4Tty8g8BTrVbqNjxaXuI2mGK25pGe1n6qFO%2ByJYUuFXsJHsn0aHraGCHpgTZUlMEqq4QkhS96%2FvsNXH1PSd%2FOhge9&X-Amz-Signature=0eb91e509fb33b0a2d85b1dcb56dfb4c72c2307c28fd16b3ff41f03292efc8e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

