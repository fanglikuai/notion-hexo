---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQBENZJ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIB0Gzh%2BGTibuCtoMdoitemnITY3CDIM7bVPufStkHaSNAiB4dLD4iAbkY4EjHmxR46r%2FPijl8uLey2ctni2Xziaf2SqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMz%2FrmW2VQFDYPau8FKtwDscgLVM1OVM%2BDsb2WvVMugqZekvaQcpr0Hu14IkTxt2LJZxq3P3cMZUQuTx3x50ZFQY2zKTcGY4ChfBhDPsKvmAO5oYQ1n17h2pb1uST9qMq97%2BrMu3dHztrV4BD3qQADP8oy4idNazeiB4uNROVXxpgnpYeaOzHML11kwbqdyv7JmzZbOErwpBAumeFC8INZ5Lybs6X%2ByqP%2F9MfqQFPCj9E8ecA0gbwpLgDIXKnz7IZNYFghjSzzSfnZt%2BZ0rDtcEUVTQG5gTJsMGVMKTIdOQ9HF%2BIbYhGiAntmcqXwuG4XgJGNJ7dbQ%2FcjjgQA5Cq2NYmOPHtMPYmK0ZMb0WTaJMifih39B8EfeukQYBrorJrDboPjlbF85C9NCOyppOwfEpmxTrYjUGOdzKg%2B4GQBVpdyOG6rvlZp8c3qkL6gIxNdHFiXl0jcGeZC1Kk9A8G8DsuTICsPGbSYMgiulSfn6SJyDm9%2BXo7xWZRWP98mh0EcWEoya%2FYClVVa4hqsoAgmhHuHU4WA0%2FnOcsBgSrm1YIfmAUpTbOsEADarwxKDy64GHoQ9sQXds43plEd2bFOz%2BNG7c%2FjDojM3plJMgFJhjgdbGkBTPwQoBAK%2FGBhFfyhxm0fwONto1XtgnC%2FQwqpOFyAY6pgF62102hdVde9vlo%2BFsCbp8i95xukRQjbB%2FHQuOToJOFz3omyiAtJ45dMwlWeYuzIOKvJMs2PI8q8U4ooTPLUz%2F49%2FmYnWBnwYNxgPPn91hQ3FK1%2Frc0ON4hvQd%2Fw4DPJV2CJu3AerDl6k9LOUNoaFHHVzbg0vYWggTpEJeHvtRHnvuOdd13OFWwSXrNZ0E4y3LYOuu5kyU%2FP%2FDDEOnjLTIEAmmpYCC&X-Amz-Signature=c742b8c41dccb4a2f0726e806ea2be0dce66c867fd0619e893d772d880c00201&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

