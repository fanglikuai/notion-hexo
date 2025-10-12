---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMNCCAZD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDwlafPsOrx0BcTLmX8J7c394PApPY%2F9rFOVBsNTPii%2FwIgBwJ9iXKP0aNmnTcB9KGSJgxvhWtcbrhBbRAh4MmXurcq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDIKZrXfQfRo6rcK55ircA7qudbeXcGXcKIRKxbj6A77%2BZ9150B9EZWlzTsU3EvE9L5KWcfjWGXAVcMHfgDd%2B8%2FLp6piJ7xHx270K2R3WvJs95SZzTaaao2weFQ%2BPKQ8%2F%2FW1hFjjFom4fwhY90sI12XNDvoSRL1om0eAE2VhnzAMohNGLXNC1b27d%2F9qKj%2BGDssxXhUnggk1Jklp2jsalATeObOrmE42ozPZ%2B3RZijyxQiVGJ%2FFbbyVR7AaLqGW98n6sXuHKIH2KphP3zBiM%2Bvvoa3v1zESJyorDFm67dlL4%2BqoC1j%2BQRREUCwMCM5UVbMDebSV9zadL4U%2Bc5joLx6%2BgS7%2F3qH%2FF9UQs08qcmoy1UDp0zq1%2FEbUDJxRFtTyTW33P0MdLhQjQfSm9Yvsuh6JzNe7%2BDB6S4R56XIgmqvrR%2BtJ5yr%2F%2FleRSjiNBucMhrz4hf%2FGNKdRdipA85zPeYaQm4Nop1p0UvwmTd66OjqsU%2FGimH5%2BCR4TJ6Wdv3XFIEPzZlN1pA2L8QtfUXvDiBylnDcVI8LX7S3UkMZLxrI9kNKWb5HW%2F8mP%2FMqcJGU47qmr08Cg%2FUluTLLeYUXSBU0OncNllCBYz3ueJNBubuE9SaLDG7kFDXNpQ5CE5gVA075jLgS37RLRx0JrJkMNq4rscGOqUBKDNzpOSukaD2hew8VKFyyOgaTGeMX5VE%2FGpNadtmnLMKRuctXouocOKU3k5xXsLA%2Bi5dxcKUudosqpydTw0dc4iE7ivH6wD6TauSRljnHzN9jroz1qy8Hx7R7a1SoVB8cYyYpX4regZ94jyV6rt0NtMyq%2B2NvHbXsUcbW0ulu%2FQd%2FWg9GZnsDF%2FHdukPTmUpbGJ6DdaBn5Jit7EbUlOI8CRojTCb&X-Amz-Signature=da4d11b6e277a2482efe660091d7ce31749f6f2a2b4aa7b6923cdb75b6461086&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

