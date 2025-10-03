---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYC4B6GI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFlC0KXWScsf2Ze67Eph7tro%2BQc9v9A16eML2xBDolciAiEA3nhT5VCvVNX0SQYgR42bAnQNe6gLeaTLj5vsa1bSfmwq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDBgwCWT%2FZ%2F%2FKzv2GSircAwQm5jaaYWkFtOnahW9kGcxQPBwIoXsC8wnwVn1ISj6bHIKkqXt6QXbsYsGf7YUYM2WlBteO9My%2FjV%2F%2FP38jWLMR7sW4pyJaRM7cvT%2FZdG3hZvx5crOpwelETw9LYorNgCqFuafXUayDNPlF89uOy6%2ByWlh1mCASMxw%2F3uuahS159xSyragckKWd8AHoPnmsudhWUo5880jlW5zCaWoawQyUiz561%2FCX733tPz%2BvT%2BE0xW7EVixoYoKlVDZCFSyubok74YyhjvM6M8ZNl9Fz6hs%2FAP7BNiUprNBWir1bEg7%2BVyoJCKb%2BWx9PItctdUp4aaq%2FX%2FW3syq7uXpq0ME5pPfnCScB4rD3%2Fn3lIvayjtkbB6GZiciG08LzStnx4GM3xKcXhZj62WTKmG8EFpk5FQoS8nGNYBqgW5s7Zwb6YR6lkDVysiUL7RH33h7tOIH%2BEnjwFq4GsMG0BYy7rNWe%2BncDAFZp2behldogc0TeQpvu1%2B6Mk3imFJIqVNMVwa%2FUJpEKuPX6ubjT0I%2FgAaISkMuwxy%2BWwe1GF8kE6jhdZYaNZTxFVNIY%2FFcLPBScGoFLADrGd9jSn1h9PkxgOD45MbWykVVeW3jb5SlO1NC69LBcAnplksJ0GGb7wdt7MP6H%2FsYGOqUB4OdiRArhAKksy1f1meP0ytJ19HzhcCPaM0cc48PchZDsgGSn2%2FLSMR4Pr8aaZX6vArr2pmQ4hq2RbCHkKDK2MCYB1rhSGFCFeuBdJPO8Os3HazvYIUXXr7I8%2BUX1lYX%2FN%2FP0ACvBnQDwfVJ4Skb2%2FdU5Fe2C%2BaTk71nrHhfZN0HBJqTcjeInkUo2jlt1DEeyw1L8yEYfVikxBt0IpCYA6zVdZPhB&X-Amz-Signature=7cb24d6cc942c0f30b5e4fb6062268b7751954502cf40e66c6accf09c85f6dc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

