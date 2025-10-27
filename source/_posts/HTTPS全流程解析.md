---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WH2QITQI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7H%2BIwT5FGKC9HNwtzin6S1t%2BZ%2B4mPI7orrckE5g6G%2FwIgVduZoWjNk%2FDop%2Blyg1Bj382Ml6PbsUNmXebEtC7fXxIqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFIYPEIZAXhjeFl9wyrcA4gI2hs0kLDr8FR7dJcGOphASR36eN0%2FQtowK7%2B4e7bJgQu%2FB1UJV8aMTuPEUcFk0Xh8U2hdOBQDcerrnQY49pLRON0RARBbXGIfrAQg6tzF3B6cnXguh%2FfVSG%2BXnnw%2FoE%2F0DvQlTjPulEr5aN7z2BrJyvg3sCteul9Oo30YLorVq3npyid%2B3y3%2BAsiB04I%2BSmp4tevb2GZL0qCjtF8qoG%2BVAvnSM6kELu6yMOxHqKi5EnTkKZbg%2F8ABkALiAKIV06M09huNv7%2F39UBriK7L4ErMHoasUsWs1BtpxhPVoFzJvtEvjgIhkFzbJxvotVurD%2Fhj885xir1BdS8bVCJimb6XIZfv8zX4TYuEu4BPYOkxtHgNQWmjm%2FAThfJ2qKrUZZY3AUdZxBffLWR7O9ITt0JvEHIV9JSqUGFCgL3weU6%2FTm5MDJMfqj3KJVbhRKjDsrsvDC1cROzo816YXHEpyhw6e83E9r1xCP4rA8%2Ba4%2BLzxSqLeA3O%2BUTUFfyRRclmadPNbfqKHiAyZRQrPjyIVmr7q%2FFIwZoV840BUbIVWFF0dsswh4zUytR5UF8RtMBAlBPfa6566uEJjv%2BIWaauAbD%2Fe%2BiRfnfaBxlFbbpaiT9%2Bmwx3Nim9PKj7kHT0MJmx%2FMcGOqUBkIutZk2AU%2BYfF97Z7GUcXtaVRWqrxEGMuRaJM5v8kS6G5WcvkGikay9GUrBPTOxqBunZnu%2FicnMjXgqJjShqpfxwfJyljEKCZIdNJLUqPQzQ4V%2BC2bVgS7hh6VtZjkSkIt4x905UKt3pcJWOyqUx%2FkHdxDbx9szh4%2BmL7MhyyfwRsFM0hucO9SBnJ8IQ5cDsMAOrnxWGHt7URNp2MVJTWVZ%2BDKDO&X-Amz-Signature=8f54644bbd4ebb18b80bcbf3e01026a800a785b7bbac715649bef3652f34a805&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

