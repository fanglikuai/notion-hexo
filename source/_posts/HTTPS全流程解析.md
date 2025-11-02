---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Z44WPFL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T140111Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJGMEQCIA9lXryYO1%2BIJr7HQW5JpwKNwJudN7A5sAc%2B%2Bd04nSWxAiA%2F0qaCINAFN36y7v%2FyK2ma3TFTDFEG8TaXfE5ms9uPfSr%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIM1SIfSNWOHsdt7iCJKtwDhm%2F9D1oIS1M4o1wfq0NEFPDTQ2zchmpTbhdTLYIlUXUmRGGH7lefVDWeNzlkKTR7wwfPTb9XIGZpW9YsZXCHBxNCtu6t01eXG%2B8ErsIweo728pEvWCjnns4QO0Ahcp0TJuZ97Wu7qHhRz5vkKceNZBKfSogZrvn0NvzBdKJ5KXQsPAj%2FzTDapRwhG5ZyLynNOQ9XH9Uu351CFh9k5t%2FD74P9LCFUEwf756bVF0ekuLaRZwrWoQXlHeb7pe29MrX%2FXFmjhS4qx5fRIIZEmtDidAr48c6O0sOEAXqYmiPgNYMrahky89Ntjd9aEiqcR1Ukl5tMI%2Fq6OJrDyfQ95wAWucx0eUwUX07TaCT5dU27XmKGGA%2BM1qB3Dg6A3izhDOpADSeM7f9KmDx8wuUZggp3pFNUzb%2FFlU0rum4pV6LwSiBY7NvE7vqU8PFzWM6R6UuV2iFtAG7X%2FnL7SHBIDomcSAtgF%2BZDndU%2BZ5hIET7uo7TsAS1qIvfSVTfLf6e9m0%2BmvkDgK41bdsiJ2UFRQVvj%2F2cmM%2BHoSwnxAMdFipWqmyIeS%2FZGcKew%2BZqzjZ%2Fi2Bd825WA6KkTOnPEW3JJ3b7Qib3IgfZhEW3HCOzrWs9tFezzlQ6P64iwJWwBKJsw1O6cyAY6pgERvxuzapFn49cafC%2F3ZpXHGMLt0Ew6ehURfqNXUuw16u43EjEZuQltpxcBx2T25jj4nSSFoLxJ0REQv25cz6%2Bl0YRcoHd2xAizDsAI3esqz4IMVuaEGcmaEoas2NXeLGMsZ5RgbHEMqOr1fW2O%2F%2BNOJKimD5XCMrs4X0xYgXcpRpKRQlprC%2FhbLk7AnZUn%2FJEWg5jCMRKmTTzuRpJWYjQa2fUTWnsV&X-Amz-Signature=6e88a2c13bd596f2da88bcb108174b1e2a97ef30041450f8f12dc42d47a5f7fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

