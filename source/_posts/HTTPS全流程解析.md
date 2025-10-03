---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IT2KLBA%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKf15HoMDxjdOtqjmyld%2Bd%2Bw8xFc7v5d5o3l60QPbR0AiEA74n%2BbAGMkpHUBFbTM5KvFTdaP92xWsVaF0OedXPHiwsq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDCiKoyX0e0rjP44C6SrcAwTR0LFVCpCWVbzsRKuNks2%2BsjkNYoz2t58pWnA6dQxgpnYfweWBVUVB5IjqWmmJ9TZ7D1TWuDzWTrYD4GS2AIUbcrYWINeAziIRUMoaeBchJCGfUbGeSJQo8qfn26LLWZW1FdhYJTUUjo0l9Gg1tZ1b0xpi3fF3AkQf62qJb84cvdqkFY86z6MXaDmGf7tCxUkaAJJ4Henb72O1IUJK78P6W4gjLhKxUy4AIndiPPB780QtZJ4MRQGYxk2SfX0rg9mctNHWCyM9jHTE9U%2BOS4lrnU2aJrmEHEM6LET4FVbNdHT8TXghyMxrh6%2BY0hj3xFmbOWS8wj%2Bisl2RR4Axi%2ByfHAPJS5mUKlo8vPjKA4qoIJE%2F5mN7vPirf0V0nEEUy9dujgF%2BBrAsaMpandBP4EJB9XIY93Zv89a2ig7RChDfXalwTxEfaufJA4j1Bn1k41QXQKwvrDPeixMC9ZDMLGfUHle76ulv29KJuXRnaYfuiEhA46IxUWVC9YMt9r08d3V2OcGTBX%2FpbLyUAxpXtyEzqrDEmUDIMvM53gPGUiW9U1VXFgF6f7nvx8fyM%2FtHH8GlQxHRzT0oNdZHVrmH3FnFbBwvCpJuJpXZRk%2BxoCqpiRAuiDOe5sxW4KHEMODS%2F8YGOqUBkdGvixIrd7%2BUkiQyIlJa8pEhJ857U2KNIg9kbgvl7YoEAtPiXs2W829bg2at3oVluuACn%2FFZMSjomnawYxnbSGZ%2FzKzrzp4R1WVHC9kOM5vV6fyru7sGkyaOd103MQHcg3Y0II8R6gtTTKaUiKzgrYev4KLe6IlTTcTobPlqnGjlkGa6UqqjfcAPLIF%2BjMGphnzBwYmFypQDqC0ZBCrHruddNBZj&X-Amz-Signature=4af81f9be8365783575ca9a5eb3856641a626d4012c2245a9593defa22056356&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

