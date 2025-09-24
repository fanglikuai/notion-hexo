---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWPNB4J%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUt9UNkmlyNKeC3evLyJdtd9LKVhgaRPW255ni%2B%2B6xEwIgLPJzQ8sLzp0cKdWaIECl%2F%2BdBTN%2Fvr1v%2BxWYL0jSk8%2B4q%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDII8K5N%2FQmcDqyJ%2F%2BSrcA59GxKsMVisWpNUJmnTa2fyWa3PbY5JVeZ%2B%2B9w3FUCCV0rZCyCGJ0ea9KAm8agpAiQCOkHwWKoKWtPf8h7mfgMZEagQUdkWUzTVrO06QdAsi6lOwO%2FSXO19eQn%2BpWI95V8cHx%2B1i3%2Fyb%2F4CXH0lyH8QaJiS404EUEGyaFHU82d%2FLquc1pCan414SBWV0tpBQqt7CKfxR%2Bh5db91ejXs%2B7kckifSuUHmmSm4qQy%2FiwMHc3nxK3dZCRGLHJYdLWjg1QqJMlts080m3RCxFsjGQ9MX97vlAOjVZhsRgS%2FsdcXycy3qr44bfyofasi5j7ztnGgjHfpWd6NxPhytBXqSrmPXxfsl06myNB%2FpwqxYKEpAsi5tQX5NFyndT%2F0%2BZaiwoGil1OoempS%2F%2FalFjEJ73Wm6R8hvOr384D%2F8Ps9SmYlSkDCv2N7TE2DxPj5epIrDESUAHOSdUK8kiGbqSXfBBWLQ%2BtkHwdcu0cpfApG0XMfJIgQSRUXc9vBKpA5eA5oUhLbPxsI9U5%2BPD2H9BFfN9RUJgThTw39SVHIj8vrq%2B7x7FCiGF06Eo2bW968BJg7iWayUzeDhoyUUtkxZP2zk3aMfxOv8O73EBoKXvKD6MgjpNM6SgqZWTNA%2FeYKzrMP%2Bd0cYGOqUBMoyQ%2B4Y6YUKZfPGmw4wD9cDjggFchB%2F5SEWtVidSC7moo4me8jUt4FUS%2BJKTBY8nfsSU0ph9r5t0RONh7KuMoVPlIz7ojQkaOfg%2FA%2FttbUH0jW2FbJ2Q6Se42jy8AZqfYvPvCofkkTXwJpY55MKGZgIU%2F2mghfFmXdX%2FFDybuVvisDh5A%2B4t7xAQPvEg49Y6RsDiXWPGAk66o8uH26e0f%2FZRDS91&X-Amz-Signature=df2b7422f15ec39e65b1e7426d01af1bc0298f4ad1317870d4ad777e61c41416&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

