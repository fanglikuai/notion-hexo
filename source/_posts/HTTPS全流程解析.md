---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OCE5DVV%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQClpqPeSTkWTf81h1YwzM9amJy%2FRt4asf9Mlrv5bD8ZoQIhAPy3Das9UUMgf%2FaUsp0Zfq2YMLSqe1ZKBiIJNTXywx5oKogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHfVlOuIWxVZIGP9Aq3AM3vdpZb7Bx6IikC3%2FM2Xn8RT2AlZzRlnvCrbWuosnbgXUnFyFsHVnUkFjoapbAnnBsLJvoYq5djR8adzs5feyTMIl79%2BpXBA6iRg6hJEf0YlGoGebDH7sXURUt7aPj6TFWsmL82U0GJYO3fH6xdy%2Br2lsmR5Q9oW%2Fo1eCOcjjddMtG7fgNlGIZyQdwvJbYN4K73k%2FKXwLkdeL%2FCV4lJ43tffiqjxg2XaesVjExyJCWvZv8ABSsAFGkxv1d8pMvb29QsBj9uZTGkber%2FKpG9XaGMqrghoe4BRy5nghrKkrSMtgvEgFzE8uZB5KMffTjx%2BC0DGo785S3S6%2FtRrEy%2BNx1KmIKjGK01b8k946qTj7F5tuWpHnN7To9Jtc%2Bhysw762z%2Fyd8T5d%2F4ShTXbuM1xJY2AqHCu2UTkXjwQnwiV4a%2F39TuslVcvINQQ1bMn%2Bog2Ut5m5705TwY1QnixrihmiBNPMgBCGmLK5sGaN4Ymi48eC6gZ7zKruj9CMcRAC%2F00dqqA2WEXvQfxlnLM%2BKKJUqcgwxn09RndEZ1bXm4zpEYF5rDDw53WYhu8XjHjTHKZZ86YhpJVagBfaZ7z7R5Bt4Tyt%2F6326cbnyiSSZ0PNrzwUCq3TbR2C2zV9hVDD%2F0vXIBjqkARnbUGYt98hqB2aM45MubTe5DZ8iuBeVthXCfOhUAemB5QC%2B8z%2FDsERJOZa3SY3gsnH6TYT0pL%2FA8vHYjyiTD5zsYO7cp9qxbcZKZqFTRDUD2uZNXvDIslkVKtmqy0ym7NeIad3Rraa11NUYTSDfdm5%2FBDcXVzd9xJguK0dg%2FDd1qLFqrs3iiYr6Yz2CQby2VF6ohIibg2eQP84Y41LFqB4PClft&X-Amz-Signature=fa5d88914883f49c11894c52e499a0a725cd018358a025de0d7b14ad86751678&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

