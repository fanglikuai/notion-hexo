---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626O2BX5A%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCM5Oaiku3FZbfGRD4Tvz%2FUxEpppEE6gAIj47snS1OTKgIgSheOCYIaSVV%2B0vUnN6mat5zFH%2BqDEJxu4TGdaJ%2FKMLIqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBxVuGPxJ5OJTO55vircA242ASJd3crMINoRtYQwAhsXTUshnFIye7fH0a8vBM29vcXYkThvCFw%2BVqk8BB3giWSpYO%2Fk%2FkTkGEGaf2MJc2GzAgNPPpFkGP%2B%2FOLzKoMzSwFMCi4FwE27X2BWaXPd%2BbzABOb8%2FeGKNReKtYj2Lm6q9GTUopcDTgFWMSGTtsfyFaTkNzlIPiE3hCeuHlT9PyWqkzzR4DSOhyXJhOqJrPa2Dqrxes426Q6vaqe8C5er2wklWsVI64ciRgPvYVBssnALKV%2FOK%2B7aALPsgbB06Wx3%2Fa1UIua%2BCvn8W3XdpJ4BGhFkSqczK0tcaSnOXwYkwK8d%2BIqzdbqsI8iKUep0MiBKwKCKkC9SdHEGvA9qFZCOxH3loB3%2Bp%2B1nl1Cfx%2B2A%2FjO3NP2mr3A58jm0gBuzmf0o10UDpPnuARHDjqov%2BMQEZXNG7T5lGA4nDaVbI%2Fk2VvZ3ihC4f9WiN%2Bi0Q68W8%2BFsKA0hOHzXVLeK1VwVrz2AFqfkCXEx6C35423sdqpJW4DVX9a%2F%2FVdMzbeKVMVKpwA9vyrmtQve0M7nIm3zUdpYqbBVBkujpwx%2BHZaMQ58pYkM5JRRu9yA6G1ujcJveYUVQsoTm3qX%2F4WUiP%2BBw%2BPXjF%2FjiHeXM4uITIL9VVMPyLuMYGOqUBvfkznWD4RDo5GIrYw0JBAQTxXLI1XKPNZ7vCFueRo7dOND8yjTgL66ZyEUZr7q904Ndo3NmcJoBn7et75PHcjm7EY%2BFqJObR0nzj1gxMQULZdhd%2FgM6aPKG2BeTEXIYL9Szb7cwVMMICsu9bc6ZT7qShfh8GuSX5jczZdu68xJzywReUVBa7wd8XTTMDRAChxMaCVReSitAnD8gosGKEz6E%2FFjFA&X-Amz-Signature=c847232d81fd00c1ad21f15f87f689fd89b45bff3a3eb5a2c5f7ae92b3168dcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

