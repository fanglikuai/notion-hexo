---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KJU7KDP%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCICURU%2BtS7MA6c3ki7eFwWWs9EHa9jQnv8hUZE3a7jsqsAiBmNMhfe2s%2BuANCvUH2dz4LPHiu1dmjg1ASJ4kCmsyhxyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhwW4LoZTQIHYFwklKtwDvWstsU3G67zFA3q%2Bh7TZGQmyqsocg2oiT9PzSuK2ynOLI8cVgyJKQO%2BOksDFlSdfbnLBLzak8uhxdAu%2FVTCiTbw5ZkXfo9hLJ8PuPjkiPmKRrjhUo%2Bjc8p1QZjmyBT6f1Il6eCLyEMbuP9LCG%2BdMCXxTDKl9uqBlaaTZaVGN1IIlq9HGec5G4hlkYkDE116CjI2kVlIY60l5te64B%2Bk36IBPsXRNvC0g9jwTCAHWldmf%2BKC8tyq9FA89JFb%2FAlhimaV43%2FbtT1misdzOI7ePIeT5R1blP0QaPg%2FBRFttcgZ4j%2FtFUeH%2FX%2Bw0UmyAK95qnZvswqFZHT8DpdYHPkSph4yeQRxPDsuFdVMJx2BqU4zSK70iCrlUYS4RaZbn%2FNP4TW8WraXTaza5Ds7v%2Fq%2BiTO4CCtallRuOBaVPrhor4qiyCdL7SIwDS594gRLGCN0Y7%2FSDUKHTyCLjrUXyWuuwtJeQJk574tlT%2BtxIHFoK%2FozptsL4NqkC8QbdVXp8zjPGvFqwKP67wT%2Fwn7cu9Ay7TM0tn9aEL1jWyyGWMBvBirZM7GTDdKZIm9l2NBkJb023RSm2yH0HXnFeuKhToP7Speom6k2YNeiVD3fQY4OJHdY%2BFpH9ghZYK%2FgOC%2Bow%2FeWjxwY6pgGDCBuPM1RyNE0Co58zgJiT771rDYzXKSqAv7mVgUrJ9YzFvFE1CjecDQGpH4Sb4un8oWdguF8b08ylaqFzbmtJb1S97SDlnI3yKVD59C6uzgS47L4mra%2BgT0Ase4hOhVytJq8XeP3blYj%2Bz1qwGUmfNn7OfGpwU8UYiNKPY9qsno9%2F3F9jyMBaLe1z%2FZ6wuZqERWkcwCjlKEMX3THYbmq2p6%2F74O4G&X-Amz-Signature=79839988a45784ca94f96e332a83aaf773bec74aa240e7c584967fa0873a2ef7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

