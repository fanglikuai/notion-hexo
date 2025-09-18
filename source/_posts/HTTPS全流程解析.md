---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKH56EXX%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIAiSxzsK%2FGhkO3oha3aI0giaptCG0AcexDXiCPS8LY8XAiEA%2B6nx4MGGugSsV%2BP1YRkQhARTE9MCwL2eoDjG62PhNr4qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDItIzoIT1wP1yNJzFyrcAywu1%2FfjFyEsk8EpwnBJe19FUZQEVoaxsJXOFoy5Yn2ONBw0GgR1wh909kqf5WJH40YUwISGsttioBhOql2nTtevHMXY0xafhRCrWM28HCGHwA0MRT1sok2%2F2UgvpQi0%2BnNfx8WJ%2Fm4suo7xngQIKjkjn5ZbHqIlk%2FxHNp3vttVwc3g42%2BAaOWf2cFxtJTHWXQPP0xbOj3voEPm3cOnmADTULu71bxy2aVQWqK5%2FaI91R%2B7Ct4PLmz2R1wngTjIRKMyo4TEjnopNaaaz4A0X8hzQOq6PrSQP5nvKP5dxX%2B%2FO4pozTKJlxPymfA7seG9ONXUJTFyiMtRYHsLDcarcVgy%2BTQxQD5KAlnMzg112DYcik%2FRAAQINM3DRpI1L77P4rVEJjhYdTWAlk2O3YSHjE4kvOET%2BzCaVxreDG1jOPEDNFeWr1D2AB4TxEBwkXrxHaw4Dl4u8on44AlPhGhRovQs4YAjHTsHxtI%2BZjJglOroLVuvg6%2FMgQEXMHhB55T0FTLn7C7QfV%2B5nLN9Cpuob8GVRSi2gs3TXJsKvxze8mJELHFHcCzo8cLduIVFHhNwsjnaZfx6Akm50sqHMoMjc%2F5dFpMdUEJsayYt5TaFjrHw4Gh8mRS7rNH0bWqQCMOO5rsYGOqUB2CsKjyn9E9orfBbseViAcyxbzkJqgs7qglw1eDpRu1Tv06nc2dtWvYcBkF4tDuv71yoYBdFbgpkscgZwHZ%2FpOk3DPZzPNfKq%2FUDWWQrfszuv3U48yQIUpMrtW%2BzoxkfCWOiRWHFPRq9m5fWo%2BVftNeIvQa0NUv3h2cvpFRbcCavx9MsApvvwBsjf6OF8mY5GRYehEYgk1o1p8B2Ec85u6HS9jyXS&X-Amz-Signature=1fd9ac917ec58a475c0d2059c1f22eed1d0c43c95d33418bda60cfacd4da5f5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

