---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWS7Y4V5%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJHMEUCIQD%2BgBdTRo8nlRtZnCkl6x5L2HX%2BoqbPQqkDbgQpD9sGUwIgErgAlPfEqLLJKpgIlTbF9YUbThb9MhRH2ipgfLshKK0qiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCga%2BsN3hsZYFzEXFircA3rQwz4XnenZmhOrPfcDv1HyERaZY0AgKoANdA5wbV02FDAqXPfvCI%2FEKXIxxmfKqcAf0YojVXB3Ax7MKV0BljGaTdnhUbvuqDKqm4VMlvdg70KPULF8YKtYjzUL4TUd7ih6gDbPYSpmMOKi43LRAFOiRg4vesSM2Za5k77j9M7vVBNoV984roAVrvko86utwfz%2B%2FQR9s7F9ri35nA9QFY6NKBgBODd4yiEB8qo1PVCYSYg7kLV5Fpz%2B4ZFtWHMrBqzSzlCg9aElDntpIbKy7sP8lCklrG77VXUTSojbxENIYLKxoQCNr%2Brvn6eYr7fgT49mWzvZlhngYG7KQpCZ%2B8dGoa%2Fj0STZ5s1hZ5v9zjnatTvg65UfWcxoutWAworYQYZww23NfA3VijW%2FrV6z9XCUfjACRKLIDrctFC8kuE5JQZ9zGSHgfbSxgBhu23wBI6esx9ufnplzPMjroQJWR0%2F3sKkhN752s0Nn%2BTFTAA%2FFofpYoajlNPI1lZ6F%2BN47QyzoCKx7ok3EiJswKxYsgH%2FBZF4ZZvV%2FOGeOoCuR78a%2BYWJbyXnCytZNQzdsI%2BdOXeVv5VgvSDzNzSUeDY2frX3bbA0%2BpZormKHJ2P8JBjthPqE8g3zUDLOh6WmbMIrX78YGOqUBuxnuxoXrvbc0hXtSyzWmWUXHJ7O5ZDMlkVHQYUnl1nrp8qdg%2FZ9kK5PBHLhOPf3Vpm049H8hEDoX6zpSkWoakw1MTtb0fNdYHBy7hvhDjejCmWDqpbFJivkFgLrpHuSoWdf3hE7orivedKaueoP6bcJ%2Fx7zkD4VuD7tJJl16WFCmDMf35NJn1v1BYsX9tbag43dAWP%2B7Xidvamnq6%2Ba3UYJOjlxo&X-Amz-Signature=21a8b812c057708389a9feae19147786d5faf8121b21dac885d1034c1c59d98e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

