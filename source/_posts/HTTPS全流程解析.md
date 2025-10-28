---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5MHL4QJ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T190118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQDE8VIUdVsYx4EwYd4vK0SqXCSKc2%2B%2BEiqYczT06G2x8QIhAKNfz13A%2B9hXlZ82RMvN7V43fvJDzSnLzvUUqjxOE%2BoPKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyb9Sl1tNiqr%2BYl6tUq3AMlcdugf34IxxAB6hF7YzaSIxhVr1QalzOgwTUyPPhsf0CYUaaVaNRXHLyMEvFek%2BAq24UFGvjdGT9yICg8zr1lAtpmqZUjGrnI%2BfCxYtLTyPQWZYwd6pAQFYuZ%2BZcEPbmEKSZs6MQpOCcZ%2FUzuiwSjJqtQZ08RMc7A8Qr7tjcINJseYz2kYnAGrtxASv9QniZzGjKF6j%2BDoqT9s%2FO2jMG6YopAB7O0ApmrDZfI9FGMIKi%2FdRdK3g11EFphwegaz7dd3RrLhv4aK5okUWyvj5IX6GNsj%2F99grMsO8bUpnutNoGeYBDqecVzK3epqrfUuQHxdfyV94Tq1GOycarlJ1x1gH0Y2hGFqmx07g2lS4xkcgfYzlKyTz1gYLanN2v1VALPrnxx1Kng8JH8R6E72nLXK2lFqsaLg50tlIPjnGbcl54vHzcR1zY6b3uEl%2FWrQzEk9YPAiS9m6iHVFRZE3cz1GOl4B34XXxGIqHPQFvGQO5T6ugD8bUQHisBD7pEaBziQJWOsJW2IqUlLy%2BTjeiGAGwkHp2Xq7JBdSkQavkr1J8YMFZZgypM3SCKT8mYXOvIE92IPLAESs4PSKmv0ZooVSU%2Fjo61pAtqasAflmGNUdFWi99%2BGnRANj6b9SzDWmITIBjqkATFp32r6VaQyOL8VT6hPhyKnv%2Bj%2B6TaYFXlRq%2BxiVrtBRPSHIi1Ju21liSFT2DEWaESJzoVeXLc8%2Fy4KXvxUwTUkqemOUKJmPc22pU%2B1%2Fu5FFVMbD7%2FYZmK5hxDPhqpewe4NEUk2vTYTYxTtdHfeunQ7NSJgtsepeSqcS1SV0xKz0kwL%2BFGaFRHnUIIXfVuzm7csKQqsc4AOfY%2Fwh4uS8PegaQWr&X-Amz-Signature=699c4608d546ac1d94443f7f3966d1cf87bbd9af5a2ec382f15b2cd94da9c62e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

