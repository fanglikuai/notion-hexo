---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J7LWPTM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBpCXT%2F1QXpQG6vq4OkmTsXDHLBDwTpo9kXvMxX97ThQAiBjFHADEqbx1vstGhBhum1ix4ziUevh%2Fzfl1dEIzWsTaCqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMc7LB83oQEr6UWQd5KtwD9Lwh7K%2FcNGKhoqn557I1%2ByaNAr4dV8578On0NTR4dq0pNrOMH1ZIFKRNVDaV42EwNb05xaxEAldJAZfmdN%2BxA%2Fgvm7ncZNDfxbzQavDW4z3%2F2Xa4MPaz9UMlTltflxcRELVDRAEcCewoabzHIA0yuZSZ1ePgZMmjUGv%2Fr9G6ZIkWRKfCPXdBlGgTc4ISlCJLGt1EcnYcAJYZ7rRLwl1CFkNdpgofYA8lbPkql2BMwQvNlRcLaob9gP1s6rHuDO9ySf4Ug9XsfdwsvQUkWWP1irhgDBjqzu3BomfhjLIAgsrk3Orz26HD5SrFToS1f3kAK4OGWxo%2F0PzCvCD2uQkYsJRSOJSvxy7Gf%2Bd8fR8zZ%2FfbNE5paBUfS5o0BiSY8ouKT6wlrsVogY2IhuRkyU2BeYGyjG9FsyDSHTCTZMn2mU%2FGAvhPmccOjDW98kHvlJzn%2BuCljHa7gxgjipdtO11unuQfuWaSeXxcUekGo2v4xVinU3EoVPtedtYz51VFqyXgVMfwiopGWMB0MzTO9grPwsAWtHQV3tV6ShF%2FLt29AY9eWlK3MkkvtOt2oCV1zAngSS%2F4ySsBUC%2BJLAGiJVNuD3%2FraPolYDgKagEGyi9TWXOC5y01OKHO5sx%2BD5gwi6uQxwY6pgG11t9QbaoTnPpDEBQbIPpZb4JZ8FnbcgnV8X0TzBtH7KAVDtEtJVnZdD2OWHYY%2FcyGVfVN3jOYvqYslmOXdkqed8EPgdo3ZER9%2FoFKlKmUFL2b8rEsNsNivpA8MgZnVHDaarTwsKPZ1FNqbD%2BeN43XogjoDeaYthGrAgc8%2FB%2FowQP%2Bp22JnMGiJHPUh9Mlp4%2FvYi3oVIjYNV8k4rjgeBdvel4tFfO%2B&X-Amz-Signature=01e96e880dc97087226b282e0fc27f9bf17cf69817e9d4ea2aa399f079537d71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

