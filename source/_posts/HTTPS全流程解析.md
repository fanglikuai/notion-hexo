---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667K75WGMX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJGMEQCIG6UZ7H%2FT9QekSQjY6rg%2FFQ%2FlBAnZaOgbLykqltit2jGAiBB2kZFDPdIlIOGZCkC309KB0RuuIcsclZGPVVojSqt4iqIBAjn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FfXJ6uorrSpyKhysKtwDF4IX16dhcK9RK9ce56QyXSlzPj58mzIPQFIEk8RW5gjIbvqIZOsajLTar%2BdMfc9Trno2Z6%2F1NzOEa8v88dG1JfQJzwR%2BxyXm2paoBOGvU1YxvZSV8G6eYiLoMa5%2FC%2B2uzt%2BLVrTScljvpT%2BJunNJhBieaSDOFTIouk63On4mTiwHA81JB%2F6%2F15d8qvvL5l6YCkwbFTWFSkiW2U5SC8b07TAlNv70US3TFbR3z%2BYbSnywyZIv4bX8h%2FNeD3YjLuUEoWZkhQNR%2FFY1s%2BPVzbjiZE7tqvIaNCm0NzrFjK86cauzdRnzR%2B%2BegX9EQFblNd6c8Rve%2BKjBnrMTZfpoKYrkomhwLl8dArckvEg%2FsGlQn3iLw8Cwyno0eykH1ADKDh0fRy5Gqv7HYxvklNyLFxFMoRZYoJrVsHGzWic8hu8E%2BrPS%2FpqtYIocRZOkoIEvkdJNFlUHedwsm%2FKJnKTFioufvgrQwS9aIaw%2FH3gFs2JdBIsetTdh0tIJq2ORydSTSb1VMqpx0PARlKIpea77ab%2FCEj2oJTIX98gHb%2FbaexfOJ5ykgc0wimJgSL0lm4pYkTXuP36YypiHBOjbw9H%2FJ8F%2FS7I5qJSw3eVj6uSmkc2QZ5a8th82FBB%2Bj2mhM1owtIa5xgY6pgGKoFpYdBonDOvUcUeWGih2LWF4N7lzkAqHyquaZJYI9CpuNDtyoC%2Fh8DoUGSq2V00dzv2Wfe14TyeZNKmG1NViLS8MhnzOCxY6DNaxHg%2F4wMZVjX%2FJlbusQgp83s%2FwccDBA1j907Va7MDQ%2Bowa69Bq%2BeXzw1x81GTXmGYZsCC176Bk7s5vT1eBp0%2BXN%2BCrMff%2B8nWeukK5cNEm6jTtJoxMGIMC0Svi&X-Amz-Signature=c3e3c2b058f81f5383a550cdaf1baba2ffccf438fa72868c747086cb8c802aea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

