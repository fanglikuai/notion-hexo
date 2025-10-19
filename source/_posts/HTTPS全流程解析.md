---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHASA4HN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIF69D%2BYfvbp6DplB9Q6JyiQNpvjA3YEL2DkYiJpasOO1AiBXfH%2Fk1ZAW%2BGCwqzqOAyPW%2F0FPZhp2UOPvw2SpfodrOSqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeR4YkxBk7TLmr2nAKtwDO%2BU4an353OPD2uzZEjQBWgBeMppiVw9sPDMKKcR2RUuSCQD5Iyk1gjeSbibdhU9mY%2FkokWBdl3tmEur0BxkQaJREDfVWSsw8uc1ez%2F8BchMOy5ZLG%2BC2LdcWqresSQJ6OIvxPHfmg2KNeqDXtCbgYdi1XPVHtkjowZaRYwpY98jUD%2BncxBWN5eT13td%2BmVlEMm0kMPMNNze%2B%2BW8pX8BXmvU7Xn4uQlhOsZxt4wae7lB4jIRWBFkwG7M8tFYXWQeG%2FkTPRd%2Bj%2F5AkpoC7a%2BpcGAsTry6uYjR1CAsi8NJ5R88DZVZq%2FkWzW67YAXOolaX6vL3FPR3Q8VeNhsVCM%2Fi7fceABGjQGMKX6RKZ6DSmAC868%2F%2FaJkYWLyf%2Fi%2BGb%2BAVmyPICj4zqUVmu%2BF0Lp4%2FkU1hywJpvdnS9BOiR%2BpZ9v7OXC%2B4KHgJAKtUzf4GD0cCO1clwtvMrC0fXoveI5lFeLCNX12c2U3edkA5BvUQ8p%2F9eUJyNqqaIl5ck98Go9B8Uzr1VvVns1CpuBFfSKB3D8PO5PeTlNEUvifaK2fsXkEsaZ9rVW6ATpWTGNIQ7aLxpEINvSyLc7kWzVt0V0OmysT6zBd7%2FQA1leX6%2BqU3cFSiR7WnNq1PuV08eJ3Uw%2BZjUxwY6pgH0KCPx5u3qNCQEkoypay00jhESdjjfFlfzMcCLLMMeqnbawhUrGd9XFDsjz%2Bvs5QmcEPbavz40BqolLvwyL34thhfWUqC%2F0cUKDKC1PwdoXHwdBVomeqAMLn3%2Fzb%2F1ID99Jp4sXbjnRm%2Be0K9N5jSSGve%2FfwmjmksOCodlr0tupRaXyWpn%2Fb9bqd8j64vwRn1h2Gp15PgApe9%2BF9UwQQvIs0ciOghm&X-Amz-Signature=4e27d96c3b089375365eecb4407da03d58c3771e4bda8779928e5b22042a9642&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

