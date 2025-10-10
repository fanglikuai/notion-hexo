---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVF5FZLX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T150119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJGMEQCIAFtl8Axq7vH1TLcP8WvBt3umu6tT4nM5YYO94LqZbBJAiBPY98jLuzPzO8pDvP4pGWsq2Jvh4Tct9%2Begrxk0MtcgSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCV7Z99yjNQTnZFxTKtwDKmk%2F6RsQGsEDGSaqd9zce%2FIAUo7UCiZoqGJ6jA5lPoYbxxUKxLhNqNGVob4sFvexf7kVZoZWNqmhY46v1W%2BCNL2bCtFRN6y3g%2FeBoa3lPK0y6niytcv6gHkxYnvm1tBfQs68yZNcrfpUbHJxDYffOH4qy4nNXjqQka0Go%2FLO7G80WHIPjNYw7Jqxkft6IoOiwvPYzPwyeMqZ3cfPbAXCc2CDmMRCPdEtVOjqOum57JS7dO5nk%2Fdhs9QYQUxj91cEmhsdDfUAv3ROqRzl6SlFj6EBSTyVBu5%2FgDW%2FrKQrMQ%2Bu1A8JATnWjrhHsbKKSI%2BdoERBcxh8sFxt5MTq2Ft4rz%2FIS19hJd6a31FSIE6LZDXjIg7xsuc18shjLF3MHkO6h0NR3U3rrqMHRlfHR4GgDigmUo4Xv6fLn1m1CS9QDXs4T8El%2FUFu523z690cmDOB8rt%2BNRzGsp9b7wvNTZ3ln0R9aXYJD4ard45uWf7yO5207tduF%2FO%2B3Mt%2Fpfh%2FQXbGQIACSoyx3lcdENvCJ%2F4z%2BJ3SFONHjghitqYbtoDI%2BkNMTGacTXsOCmmhxcSOJA0BHVMqo8%2FnNHq83GcpWHnsK%2FJe8GL0%2FDovQUY8yQMpRaaEywzLh2ZwQomh8GUwvaakxwY6pgHJwWhMFata47EpTpss3rV4rDgtjUxMtz7BpAr8X%2BLIw1k982AstW1CqLY6qwM3P3vrv3xv0H5B72Z%2BFtq8s3RFUwjhqHrTuCQGprvbiZ6%2FKbbvDJNN52YrNta6zelml9GeKOmFBq1g3TwVtBOHynTdyOsphJjVNyjn8%2FU2ve7RBGYIEEIkNwi7NvSUHtbWyEofasjvPfHpN%2FTA32wMwfZFaCup95Gg&X-Amz-Signature=30091ef038fbdf26ef6cdec01c450212329eba19321f465bae5382762ca7d53f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

