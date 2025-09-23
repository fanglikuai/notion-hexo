---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GYVDTO7%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJPX2nlIfpx5pyb28ydm7y7ek00M3ldaQuKqdmuaLAfgIgAng%2BCPs5HDpATGdPGUn2hPpXKdsN3sJ8fPOVUweWKwsq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDMddsRw1FJFIqPwtzyrcA2x1ep%2BxxjaNQr04Gr9KtxaRJOHn2MNTlhBsleP7i0NDsLBK0GsI%2F59aAPLhV4oHd4e7DFc1DRqpVp%2BYouP6V5bDMjEcPEZzi6jjXBf8zj%2F7XMGwsV4i7azMIUKu6vuz1embjrh%2BXOhEjqOkV2Vz%2BvtWodsqGI7D%2B3OWk9%2BHQE3%2FcFmnJqH86pnUEMgM0pNK6BSEpJAN4nmaOm6XpTHlskrTXOcskatYcrSi3MSjZgYO95HmsOR7wWHkLJabS7a%2FUTAb2WDQSVUINS%2BGGA1h%2FfFilDSoi73FRmu1whxg5MR8LHqWYZRYUdvBTMb1tS0tkCdEa7xzoM9N8qGVKu3IIgK6EKnPmkLQ%2FuIHbzxwod1zSyQGSwJvqZaNK3%2FegJ5yk1fNBr8NETv5kqZmtu2a47Z5nH9Ng1d3OvPBO4KgfO0caNBBLTBL1UVbjqlGYJQBo6nvVHwaMaTIm%2B%2Fl7geTuZddfUY%2BLx%2BQfPRjPopT%2Fo7AOSk%2F%2F9TiwG1qXyW5NXTmbGMP%2Bw2N1Fs798qU%2FdtgNyG0DWfEZWObh0VjnZ%2FiLe6w29gElCMMGcAYiSRTmgrKmPIhRfZ%2Fzfwk9bkLI9X4zSqgQfL9BIGwI4tmBqX6K6XVoGy%2F0Vg0WaiTKTiIMOPzx8YGOqUBo0b%2F8xG0kMspoVtti03cC0mVlNVvEVrH9H%2B1heYuctVqUCo0NdnoieV18Wb3OyJQrCA8LkSf3ZB%2FdBHb7tgKAYX8Z7QzkiV9o1%2B20TaLIXDI%2FCb%2FUHdztMD1Dl88TODer4glkwR%2BBEAGbLbWYzlVjFKn02wfGIO255Ig8CHIqQIKkmA8LmaOWRdH4GyZ7sbkQ1xd9QZ3xw2LkYVcqX3Ad%2FB7Lmdx&X-Amz-Signature=4ce1eb99b26782e7c0886685c39ce1c24d445302c3ec225eb35b562eb779b6cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

