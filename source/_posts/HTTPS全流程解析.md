---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DEXLHFS%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQDDSAU%2FwzFjxt5S%2BV%2FcexH21%2BYb41jq2Aokh6MT7pqpEAIhAPCII%2Fzktvn9pDp7RQInHGUve7kGBaim7HUFMdRTwvnUKv8DCCgQABoMNjM3NDIzMTgzODA1Igx%2F%2FWavk%2B27W3YsRqoq3AO9mP%2B%2B9vm7HGshMvsR3AHQD3B4xQHbl8thFdUyiuX16yqM3CEcnZGWFcabnZhVzXa4cU4H%2FHwrXpQ4wdKhjIB8a0oIdPMzxR1BnXLNpdD0%2BtA7ZlovodbjA4MPZcua7APUSxYiqRgVgWubZWvhM8V34lQjUm5XZlp%2B5vSkaDU2CVUtHukMXCf0SrqL1v7ELksSyt3nMOVKtCMyMf3twvJlsdw3V5pkpbhJEjrHnqHOd9xtp9a8nrEkZAcOKYY8yuTQCpkf2sN1weJk1UWavPBSIG4KkNch7FQRiUCFfK%2BsCb3hVDnFGwm9JbEKL9QrMhdA5QfagW9wUdHd9MHwRjHgy7TjeKHp8S77AweE9G9mnnsJh7y1Z%2FZ8V1kDB5%2BomfuZ7tZiSWjwITIgUVAiKp%2Bks4nsHKKi26QTWA3OBbDFNCutuIr1Z%2B92QPUO408YAy3uq8ckaoptYGDAomEUfnsxbyqXpx1JRu83xpbtelbqO9lzzikLYKMsTGg%2F6Y2xQGFgjQT3iqQn2AYknqnl8PaYvVoE%2BkHsyex%2BmkJPA%2FYdOEoC2UhxxWf8XiYieWEGjD2szky0nv0%2BC0pwBjAGTbxa%2BIYycttzNnEjkKDVmNd03572i2pwy7xY85jZ9jDsiM%2FIBjqkAYyUKK3fgRVy1qg6CixkIL564gfp4UsDmUff4fQA6JiiG6X07EgDBFqIjgdwe9M0YsO0zV8hflthbqZ%2FG8Z0b%2FUWGsdKGjqNLVMVOG3xErwaAig6FyXsJcnv7%2BWZh1sn5ylpqIu%2BUdhLL4P0RCD6FhPj1DN7ueaSRy5XhpVFlC3H4jSIHikGwqDq656BHlx2B7cgGEal72dHOLNfNcAIM2Y8JTK0&X-Amz-Signature=9a3d9f1ff89e0983e2292e9dd23bfd406ecb020ad0ba8554a28b9edcc88dc76a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

