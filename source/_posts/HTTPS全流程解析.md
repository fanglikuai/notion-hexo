---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWERFRW2%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX6t4b0%2FFDbdh%2BmVpCsMdwJPs2EIi0YbN32j6LJVnYPAiBxWO2MpHX5iIAPR7Jip6aSfo0yrQR%2BiNwtndCZ%2Fc45nCr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMI62%2BJ6cox4jng8vxKtwDslWrWtQQdvHQrjvmSxcDMC62L7JWrGEi0%2Fwo%2BLmepUHhgNiA1o%2BBBQP1Yi3sVlhznKtGZB9oavPZpUzCsjxW8zJGdp0p%2F6uX6zOUSpL2HVnAY1t%2Ffl%2FuVn7IkOVP9JUOStIZIbIijG6gYU0MT3X2kjZryXOUQGKjIFqZIMxyXVB46SbSzgJm%2BgXSjzWScXmMItibZ2KU7hxhFWGKDpljEErsIN%2F2Rs6D1Ptmp%2BTDbFG62hLR%2FM5XL4th1FWQ%2BsZqOTW%2F7KaimBeoXnHHC5NVoe2l%2BG6943cL8e66qhir6%2BjOURwNWjS4VSJyCkmGV3uKIfJNJKRbV%2FyslzJorQ%2BM9K6CEG7Qz%2BWmpTNqZyNJFRLmyfJn2WIOnlYBWX6P2Mr9QkqR4UGegPHWkh67Zo3g5vm1mspDG%2FMGS5GT4TVxDdwtrlQ%2BzZxLjVwBtPvsL9BLcD71GhsaHq8VprjZTLoH457xFwo63eW%2FhLjSYevg7bGV3SRnNgEtAgYXXw4uwIxnBBiv19W9HGIu2%2F2miZWdzr6%2BJE5nNw%2BAsOffFXfUGas%2BWxa824y5KwU%2BTP5b%2FFeyE1K0sl7wqLmrqvLZr%2BK3yQbS5X682VIq4SgQJ620H91FOj6L%2BK6BnriEFV4w2PjPxgY6pgERkknxieXicX5f1k0n%2BAQ3wmmE%2BNUKiFZwRRdRwiWjF6PlfTCQdBW3KWoZrFXDhC0quh4AhfXQUCpvCcFujvhqJx3usyW2%2FWOBwd9o2qLJNVH0hv8lIrxB8KdkR%2BCwlz%2F%2BDZclRomN%2BiJD%2Fg7fTz5qKn5NF%2FhRHLHCwnZN08WS30NN1UdaSlWnB5%2FIfwruUzIIXPSUhOqaFHk%2B8CdclmX8xDrtcrbp&X-Amz-Signature=6eb26ed1d859869ba59bab327ee923bf7b209d3bc761b5f1192b07e8c4c612e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

