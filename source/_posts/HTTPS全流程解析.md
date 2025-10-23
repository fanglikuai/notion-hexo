---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673NX6475%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T160059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFrOp1Mf78ctew1oIpSkWJwJiamwt8Hka%2B7X51QmY2kqAiAY9IucjdXF0EY5Ozpa%2BVxdBEXBCnxEZtch3QEn%2F3g9TCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMUp%2Fni9%2F%2B7UEq0DEKKtwDWWi0Y2KkxGbUHCvTN%2Bi4eUkfRnUWIrC%2F3OrXwVojlhcVQ7F5B0I7UKANQia3AVH6cYzkqIvoBFjmav7YK9AO9DYilbsvVqWoO41Qy5W0wKgWA9Tmaa3mM7RRZ%2BRXWg2%2B6x8mIH9a%2Fy%2BMe6lHBEv8FMgK4IL%2BZHBs2ecW5pC03p0%2F6OXQygEAFUoIcyLkQT7ySaVOLx%2Fd1JrBuM7qyCJ2lfDYgSZdXfCL%2FGFuqwH5X4nx6qZ64ANEk8V06eOb687m5bgvSoxi5vDKdQCemSzF7fszfqGkc%2BLTDDtVrnasK8QelFqoM%2BQp6jccItIc%2Beg9uPRxPsLF2tz8IWulVJCfovMj6Lar6TuYn6TaTFr%2F39M3zbci1XCKOUrqcBJZeMFoKZ4RU3%2F3pK2EV%2FIaiLAcCifdYkRtGFevmmyGdB%2Buhsz2zSJtDW46PJaeQSWy1zcyEBmhidc3bjFhB2vJhp8qk4T8mDqqHdfCpWac%2BurPPT5ea9EuPZxaHa1He5IDnMAl3s76ggYMqyPRb0M%2Ftrw6kOPY2p2orFtqmVdRjcDnGyVYfLhaxpKrrNsHNnTk8dbyELnYByLUCpQjpl2l6NquLN9eA9F2wjgEIcQjP8UZrqLYdSmkixDKHvUudfEw7pzpxwY6pgH2JJ6iWR6f0OkGqmGmhRElZkAkpRSgydge1G4DQNQk9Ph7DjjibKRIw4006rlGADbXoGsVRzZw69AaewkiGBy%2BEaDZScj6XdfMFFSMmwUdxn%2FlTxdTTO4nMFKB7qIU3oq%2F5VaEM8mQbSw2dluJ25ccUdAoS%2FNDMURkwvfAuB1caF0aNdH3hJGhfuwd9T6mM5DN4IpAGYcR98xO3S2uZ%2BotF6oYkdi0&X-Amz-Signature=f7e63835646c508e6d20acb927d242fc03221181832bc4809528bab81dbf7617&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

