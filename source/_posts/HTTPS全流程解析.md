---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHSINTRM%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIGkWs1tADg7E%2BHJym6UzbvQd%2Bf1q%2B9OmEpem6NEYjNp9AiBkrtp430P99Q6NOVSOl7ATBn%2Fl03wgth8WMDNcBn1UZSr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMApegjDmo6KuuH%2FcMKtwDc%2B4o7rfLe7Q1VJBzMc65X6gnxPYYCuKeG9u7MGleA09DaCPYTvZMNZMMckqz%2FMAi1O9Wb0qiOtsCUdf7%2BhVWhrN7AYZA1T%2BGJdlF6PUy7HxqCUkvIcGhiVa2pLsBrbifdo9BKL9rP074kiJqFuDDRjqX%2FHPBicysatSE6MAZ6i177gLkkFDxyDxKz79FmDE%2Fxp36EvehuC9RmkQc9gPftXas2YSZ35P0fKn5lCmurWA%2FaL5E%2F7TvKKDl9bzOq8FW15aUkq%2FgEyfULduwB68wjy9HmS%2BMKaNH0AnBWhyODP4SiT1QMYrawkZ%2BeWYKYctKklvVoQo4gTIUdprAVZMJvvj%2F2quroVpfE0JNfOo0D%2FKphhhoZjxwWh%2FyrJeFJL3L5XroJBgj6mVfZRYRliV3%2FV2MR3WfLAk0wlyGOwCJRsf1k%2FOt4fN1ruzgPxtVzSuUYJn2guhwMYxMxHnuTuSh%2FrrAPektnh7s1Q9klK7EHLAGlRQvRWHjvJ40GPLYBxBqDyNTOEL65ccItVuPDq8PBlSUCB3M2dEthd7Yyz6TQzYPZj2%2FZfkKW856%2BgBmEGpbQYyz1EqLv1lfzGFvLatkAUA2eqZ7Y0yzPFgEmUbIR8aLwI7D29mNgBCrd1owu%2BDTyAY6pgEl3BQ6dIm78TCr1yiLNq%2BLrCVps2LKjgmly80RBBzTeC%2FyCzMTe8A2GG9432%2F1rzrG9wv3M2UwOf8okNgiZJl1EEv9iwc1oPYOq2HBabmE9CPtrxnTwYXows2CaMlJBjaiwQQFREjSiaJlgD5u3V1T2mlECI1UU4OOweZQSgCCw5rs3vBL36sMyFeECUxR1dIrpOse8%2FbOQztcHUhzR3G5vqAVu1b9&X-Amz-Signature=ea8bbe7eb6f99d226c34e82bc1b7449ad5cad1e548b088c169a40a4897fc5715&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

