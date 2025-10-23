---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMZE6VG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDPQ6AEtmof3z%2BCQWS7DlCr05fS3fqB%2B0AzB0pGPzlkLgIhALNOTiPo1ayvngwVilWRRwnd0Vh3JLVBUENa6TQD9vfqKv8DCEQQABoMNjM3NDIzMTgzODA1Igx23GSK0JK4VWs48sMq3AMI7rDDdUe7LXZnuRNVb9HL9e49GXLwgbhToXZqaSqGIWHyjJt45DD25lfXuxyCy7EjT25KxWkkuh0BiyAev3QCh%2B0bq%2FMhqJQwQZiux52mX6k9Ytl99vOzfOwolKtm2DI4oInx9992MR8orYTBVsRaKxmwBcgqtpIBSDi0KncWfNfe9As38qXm3n8ATUWV7nMIBTeCccYi6uSODNfotHLzxLhzKbhoE5l2XzEPtU8tQXppY4fRntCyl9jJw76c3Cp3O%2FQVQ5HfjCb8YsPy1yS72VwpyAixO41Z3Q1NxtH%2BraeW5nBlLFbmS7vJc28tUI8F72x2K63oZ5aI34P9dRcSAc6l%2FRAWs%2BdxxziLWw2JjuVCYWONE3GV2eu8XGis6aRe4jRvuaFaz7l1TrQLPJf4aXF%2FdVNK%2F0BE7uZPXaYEjelEJ06CkBM8oC5kMQJN%2BqEkakniNoiVL3VP%2BeicXJwMqqNBy7ZRFdVqOj2IJ6rkwk9cuQkNEjrbwh5G5RM1sjJzPzFai9Ll76zeegcL%2BfuBwqKRKR%2FEZPFP7e5t4PLJZe0nQIELiEFOXl2%2F8wQLvqw11qVI%2FSDldKUBX%2FgNE6ynzkLihjsL2yQDgFH4NNj5opJmoqPwyiV7epogoDD1l%2BjHBjqkARyewTPkSiSf12QxqaV%2BuEBvpBR%2F7GGt824VAxYJOWtsvVhENIPhuqd9u9uLSTt7iy3%2BzJ4jO2DXVR%2BJAbNT%2FDGLPF2c4Q5q91OKZds76CXFBF%2FYGiXW0rhIS3UvVBJik2xmFIvwPCRa1XmL5uEPvGazihLf9wVAGjqorPehXimThDpTn5X%2BkFI9O0HTdJWCs5RJuS%2Fng08hco0bfCokE7i5Lpqu&X-Amz-Signature=3ffae0fa411d9e4df83edcd60a08cfb3ff2c9311a5d7ab5cebf5abfa1d1e726c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

