---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSU4GYTQ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDkIhgdK3gjYlbgG1RtQc%2FZrdrFV%2FUSrUMDV1Ifb%2FMyAwIgfMvrqOQXaTrU5ZTeB1Ge0s%2BTi14tg1KdGKc3n2Q3McYqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFSsoWlFgLrzxMcqUyrcAxGwBbbmMABVt3ixj8VRyLNEWlqLOtadMDogKWOggw5FhYuxtVud9gTFITJzRcvCsZJz8h4PLraigF9WrDPLAGknX13d6z59bDqx1J4AC91kiTiek%2BsSFLnazSQmtTSmpm9b9GJsKdHy9gvsPYY%2Fc2DEkkDFnRkJeBTRSo%2F%2B3Dx8z0b3eP0wz%2F7%2BM3sfCH%2FPvTMUdqY%2Fa9L7nu%2BN%2FoooAlpMp7QzSuG5Qy9bH1nkphFS1KBhjznKW%2B7txiLG0uSWKkhh3xvSWXbiikUEBZK0bM9MJTfox%2BxFKq0OyFYnk5TIIhr0x2MBDbdPMYdAZv1R0ehM3COH4dMDLsNo6Vqje%2Fcl1EcLhPx8W2Pro3BrWJHzkHgNalb6nWO8hZhsNDD0UDRRs1O1L%2FXLARpOaKBzoU0mlD%2B%2FE6mp6MYTSmWvjEPfjSc8lhqgNvMXvYdezy8ZTUHsye%2F1N%2FtRCxLRQjnweFNcuyPZLwF%2F0EiYUXgRLf4ulX34dRYmO%2FCAXoY9fh1woe5%2Ff692%2Btsr5D4Zt4XyYkZFGD%2Fd6Gb3nSkHlCU%2BotLAcWjUAc9TTWXf8KuWGVdzWTdxBwlhyIH3DCSX1HUaBsL6sBN8CKWUXFGALVZa4pFaFjG0gNxs7%2BB2Z%2FaaMJ2cicgGOqUBRTIuUajATqoI7amN7XoqJ7P4qSL1sNqSACB1879VWsNHKXWf%2BnBpL9hXlTo4ROqPcM%2BhPRIUBlivSzH6opw33CsqPWF2ewqDs2Ql6QcwIU9bAJL8B1HpXRUDPJdmYP8CVXzWZWr2b8Q5SVYe6kZ0V1n7Ts6OKCS67eLl9Rs%2FcrbQFBA%2BBBZ4dieo2558cUGxEXsGhx5diUA2WpubPKL2Uh8Uky6x&X-Amz-Signature=9e8c3af75d9dc1e5d957905fb7882ad16f9d04a5419dce9b2100a9e21f6fd36e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

