---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PKMEBNL%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCIAHINidy08HsrH6adFgVy3NoKcj4bWYwZzkveObrMflzAiAsPlm4b0I4R7MC2POmnZyYQDBbSqq2zkhJO7NjINEFISqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMox1LfDFcyEWStdLLKtwD%2F5T976hi9ySlwwtQoGcCr6zIOQ2yFvtzYBeObgrXgTTrfMO24YsYdpIcUPzE%2BpDNkzYiZpWEogDiDFH20fa1ry5G2TGG%2F6v4hxnQ%2Bxdz%2B%2BCwt0baNgKtMLPnaS8QhyX%2BZJcc0ukXuGsLXksJFZ%2FzLB8yPFz47SjvDw%2Fo9LhsefA2J7N4Z62HJ49jY3Cvu21kU5cGOVVR%2BcvWIZ1LqfnvrcEJsadBXBp8TnXKkwHDSooqn%2BGJeifUc%2BaUQkDvSu2nKN0WLjGI14xDR8XfQjD39bxQf%2Bg0oohiUOP0IDI9e6Blt4VIvSw%2BZK471%2BFyoxGs4VzZB63mOqg%2BET7YkReuNPRVjb7VorFx7LGVY2utfX9e0ek0c8uK3cdzPCBAHzSWNrSp1yyx4oo9%2FhM2pMUZj7vgoIx%2Fn6UvrjmNYhNmXPSHuon9oP%2BPpko5DwLPPHz0eeEB%2BycKlut%2F%2B1D8W9MeR6jZ3JgGyoslrnasmdAQnQ2iGtkTNy%2B21TxsKHrI4rPt30j%2BxeRpnVYRdCpmIoYgnnJ%2F4%2BdG3iR%2BqDJNHaSuPFT%2FbiPiUsixd2IPf8l%2F5xrBNEnpLJbgZjfT3qp81SBk0Amale07gfuB5i5DMcYWri2sCjtTGm2y3QLBYFMw5KPzyAY6pgGG0t9QJBTEoIihjtrcvpqQY4maYN708MdjrfWc3xP%2FV3OD%2Fared9VdPNfBM7UYOazfDkUJyc%2B7fieq5TYE76bsV1zWn%2Bkhpzak1Yy8G3VGwPTgh8JcJalsKE%2BYNLFTFRae48rKuIWdmPFz9W2L8O59TMwC7bRwbuLRdG3jPsLKk1eWIdktRVmqlYr3AzTqyVBLAUEFbFk2LCJJcax3uYm%2FRe%2BoyOMw&X-Amz-Signature=c313449447d49fabf68c4a79f1885f4e1392b505532399b2998ff33eabd74143&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

