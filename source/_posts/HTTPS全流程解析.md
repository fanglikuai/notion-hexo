---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2I56H5A%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPoRvMRoxqPDxaSYLXjjWfAXmEo5poMGiEy6Dc3LDgqAIhAIBX8FxOcRq%2BiwzD60FkZnJ7D2rcRmPIBVWISben%2BIC4Kv8DCCEQABoMNjM3NDIzMTgzODA1IgzC4TC6vBgi%2BErIODcq3AOlO18MY4cgVE3WAlHTm90TfDI3ku8LINRFlrfZ0Zeh7o1%2BX2KdEh%2B%2F48Yp5TfWUWvs6RH1yO0L3WabAmXeN%2FC5cYvkZlbgK1j6X9QY8FMZGsDhSGl8gPDLO9CplhNRj49u9uXlyT69up3%2B50kuNFYKqF%2F1hiUPzKdW1RqxtC9u78vjoBU%2FgHBGpnCUlrB4t9Fa4SjlsrhNdCITV6xYKWal4jZ7pg2GQ5oaktxVSAiRYiNkbETZy2ZWgnalBg6SurcjMBOfy3P4gnfzTN9566zcYsK76o1iAOEiJb3bnAUS6k0geu3Sa6SREQMXRH4IbcPrIQ0GSoehV340fvUGmbSTEvS57iN9DGvyt%2BdV18c6iOHkTeXEYdF%2FfaGVhUz1PPrUBv0ewJRBNw4UlVELyrJwYNb5bQdzWjF8gqlBEP1ukemK3H%2FOPnVPdoXdw1282bVTrAJ5F64H1tC%2BHquSMsk5tx3Ssv1cVnym%2BwyMDgm1Lk8%2BM3clQoM1yKGHCefViYCU4AOPCDeGXrFR6q92JvWiLCWtEmt%2B%2BCou4OZxJrjlXOCRSpdIevsp5Kq1WbhDje3QURIjspnU8t%2F8uptyJ3bMJ%2FBSKo%2FpR%2FAiY4CHjQsry8Lm2GdUNW%2F5W9O%2FkTC7%2BvbGBjqkAWPUtD4TeDtbScnPSF7nzeTMt4hOsqmfLQGBrg61XnqtbEvNUEHc0OdJlC6euJ0qGAv%2FcUc7FAVFi2UHPriTLkNXC4qi9iRi6psPpAn9ivvgW67rREoPRPwP6ggIQFBQ45yuOBO4yM5j59p1ywhfMRxbRwFd6FjDShNODdXKgNDBi4joUFPw6i9IMwv4%2BlpHvh1vjDt%2F8cnojGZ5e0wMx8Yj5wNo&X-Amz-Signature=084cf548fcccd639bd8451d1c4825bc658d93f30c617122952eed7a8d61d7a6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

