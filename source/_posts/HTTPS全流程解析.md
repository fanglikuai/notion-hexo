---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AGXEHS2%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIEUDrShfwN8EHgX%2B768Ja%2BtnSzH78CWS0UJRKjsHcHgLAiBUpofk8E7pv2V%2B4Xud4B7ouKIFtSNUxqQ38ztV9aKUNyr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMZ1m3OK27cIz6AAw5KtwDX7eYGJi0ARzMuRPaMODG2eUd4E1Ge%2FyOxQOOHdS5NC%2BnqYBakz68aDrplQVLPSM%2BO3UdM3QwlERRRmm3PkmAWMLgsfx7pkzWfUPdQQZX%2FXub48hsBaHVs5rc2SmFs7kRxEd3KVCv3IbZ%2B9qhk6CexUV4Q%2BuGSVLsT%2FWkJbH75%2FXYtRgQr6KXyj8EBgCX1vGCs9zYJpTcc3aonZ2D4kgQu0Wr6d0D8u35mLKCEDX9sDcKWhewwQS7h7PWh33Us0gXUv15yOj0EzrVAZps%2Fz6NNGTBljgvJ7oRUthzmeqgbA9rA7%2FAjkMX76l74ObYql64PXHMG90VWzDGx%2BatLDTPcx38ZoancExspU%2FJRnIZgnzbeCMQzPTGZrxdFdJ8wswGJwLFC8ufqNR0YtwNVEbBqPNV62r4Ozjy8KsfXGAygtMgjG%2BLIFdaYBbR%2FmJyZDbOAE82HV32RuqQAYFVmvpYnXQm6FzY0m9DntUc9YPt1KESLl5cEOBVz4qeMyZNXIo%2F9mUDxQuwQOtu2cISkDDPhTVzeN52pfRJInLHjRT%2FCOnmyYoklHzTIlvB4EiYiomivLvRVw%2FfgSfwkMxX8P%2BWzh3bfdser%2FXT7NT%2BVxPy5V3lBPQ7t4Vu90s7D0Uwhd%2BFyQY6pgEYsQi73Kz5mc8DIG5fZK%2BMoEt4cN7XH7g3baxN0Lesf%2B%2FCIz2UdM5iFtyB%2B0OyOjhH05caFd49LWLs8AACJQBndMlutSRcKHh2GqJu4ZBlp3SjOgLQMHEAo3%2BKXrcWD0a%2F8VIVpYrdudek7OCjTU%2BdyfhuRGmRj3%2Fxtt0qGT7hvPs9ScLhCovXTlqgx%2Fn7uVdZYvHp05IZ8KON3GvOknMPi1vBlLOb&X-Amz-Signature=248b85c1264cae7b93e66401e05834f9f98fde1b5184dfdea1ed459ae7f7bcce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

