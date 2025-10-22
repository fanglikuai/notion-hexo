---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JHTIPWB%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJGMEQCIGNmbiQLzYW2PoidPiJppT6MS3s3ImAPD9OTyIxo102iAiAfV7KoHBQnfuCdoRcpFyfTzIQU78KK4BlljfL5OiW68Cr%2FAwgxEAAaDDYzNzQyMzE4MzgwNSIML%2B4Drda%2FDrUOb%2BYYKtwDmirQ%2FewNzCoq29VKUZV9IYty2K5FqG6IMr%2Fz7biksCEAw2mLGgbRZzVMyMGTZUeM1Q7dzrr78JuRV1WraZLwKf%2BKU6G8qjFLIURh0iutLgo2ErGJSmEtDryn2Tim6t5xtkGEO8jiiCNhu4i6ga%2FXSO71pZ8ltZUAQbrj0kmlE6majUiCJitx%2Bo5H13TKnnFSeiUev6MwpeuGs%2FrETtKilwPZbw81oUOHipGSnyEViyINc1i2FsEwyTufMIiWzWWnQbo%2FrLRq9K4ayfFLbY9PbzozV5tIgHpNcFWlp%2BOoBvYzZJhyg5pq4iIuR%2FnD1Hulfg1giq8Z3JV0nqDU7qX9bUFyZgTIL6lVH4AqlXAkRdJZqBEze3ISI%2FZr0HKIqi1J2zqbnGZ74TdHNHuN8HLiQJc5MRu4v3Lo%2BQeFZ2lIx2XRUNGm%2Bn2yqhN5%2FBnhFqreq6djcSkB9XdVzIixm5PXi1NOSFuJy5AWHk5JUrcHBc1M5%2BmmXjhxtKp0nvRhpc67uwRLKFeeNEF%2BBbvUvcPMUGDZOerKn1RHZ%2BFmQllJ8gZOncn0bAIt%2BGJYp5fjSnksFxJpZmiQvZBtAkfyugn%2FRNKg5c2P96jRZiGlQSH4aMdz3VDoPRMh3AKz%2FqYw8fTjxwY6pgHb6%2Fmq2MZbQ9U14Lvu1fXSb5KkbyBmScxjbD%2BuHyVTh8XsLrstMzurzbrWKOQYPanVy1z7Y%2BhALZcpAkSWLESuBQNA0VK%2FuHmi%2BOWt7nvdsXQx1Q0F0GUBS9URwArCHLhb7AM%2F7a%2BkT8KG5Jda%2BH3LZXKczLVymSHm552gi0iZfKwGqhGqedxb4XE%2B%2BVD587AYHnhPkevqUCPeF%2Fo3ahW0n%2BdL2gJq&X-Amz-Signature=a4e66f689c149ddebefb0cd6544267a35188aa8f384fc8e26e8e087a9fa476e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

