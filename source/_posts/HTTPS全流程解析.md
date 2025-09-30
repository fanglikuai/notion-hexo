---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q574Q32N%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQC4N%2FEacmIZEnx9Ss7tHwNFfc1fZaaXP874mYY4ww2CSAIgQxAK9JqdfPWCbp2iOc8o9FOreqiI6t6NkdmfJdcfO60qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLLIFl2R3BFwNgmv5SrcA%2BMtgOp4LxseLrhAJlJVy%2BumI2Ttp2QIm%2FGpUTmyZO2itfP9Tzuej9L9KQ0M%2B1Ga6W1zS4npOFsNH80kMZFEtH5DlytGpX%2BOrAGQ4vifgDQRQu86RryxG0gwflHtIYCtXZVuRPXSEqEUao%2FZRYhUqlCDiyEX9Djx81ygbZY9fsehwALxKajLbfEu2G%2F4awSb8E2%2FrdwttBCi%2FaD1QCO5i2uf%2B7Y1US748bSl6eWEGULMpwP9%2Foo7gNXE8%2BSsGyABW3wdivkMUVZZ9VZcaAxgEVMBqQ3BrVxre6VFTIOpnD0XI8ifHv3tDSPwbQcd8w0c%2FLNUUaBBo0r2uA4gjTUtxuPBkmmo0I7ganXg7lYGmEMQjySZiEhycgawD1C%2BArfKFYefjtxuAfP5hoOrmwoBnfktXjyy%2FZ4aB2dc2wA2sFqAaqXJLV2OmHPyX9tKbX0ZYx4U2OB8S2ZcYA7eFzOfDqObIwAhb2BU1hKw%2BNPyLbp4YI8eGvczK%2FyOREQHXOcWvW7CVhia0VCAucQnhrcntC96t1d3rFu8SEHa5u85KeS13Fcg9hMXKCFOlXSuDbEeiulP6BjvzLGTaileZwhRMH7eNPwyVrHOGNh8RQSfQjML2PD3s194aZxC9MU%2FMJyG7sYGOqUB7OxL3uIWuk%2BZlbIN958gmC55FuHax7kul%2BHtInuIG%2F3lysufENumBFM4qG826qycKedSAd%2Fhzb026UCguvnywkSo8cHgrMhBAJFLENNdCJ3Gu8CIsjWLbUiHmGA7gwdpYVU43d6jYYYsQnaAxNhHNSwOxtty2b1R5aMgOX9oG2Z%2BfSkUoNFowFAypWzPDdORmg%2FEbU%2Fbbpb%2BrLa5iINGemEkAhKh&X-Amz-Signature=327163cee48c61edf5b8f7e9a57eed6fbd782385343abe0ac54e3076b357c198&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

