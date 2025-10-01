---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABGOBTP%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJGMEQCID16CfREx8QZ4mOQVnZUYKqtKKOD4xn%2BmjbLBERRQR%2F9AiAY2bSNhnQc1jU90jPi2CLexvFq7bQjfENsTlgJb36L9yr%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM6yngeFXmbCCoyFDDKtwDYJX6wHgOvqgDdZE46LR9JrPmjfN4PN8rqLeRwzPv290o9Dy6y4Iv9xqf%2FgVX0kJaPNZ6RXt4R%2F6kpCfsJV2kOSNs2B6hDnbfqgRM9BY3dNbHCR1J6ZirWiUhQdQmCQFeQKs%2BUi6RYWiwfJHU%2FGnBTaR0aElz%2BLgJL69ceS9HVoiDhyu0eD6UDz4oRYD%2BrhSJsSZvfo61hI0%2Bj%2B0H60F7YhDQkEZvl4G6PmvIdU1wVxu8KtN%2BeVpOpz2AZhGSMl1niqfG9hl0z%2FgYUy3Ke3bEGPjUdejmJWt23ByEUALHf4CwIZE%2BPApNX%2F4CjJQSDBXdx7HKI73o21fc4qh7Cu3QC5HhNEmsNb7r0Ky6bJvqt%2BFGBTsaqa9CHEDwLSmiODiI2GFh5hfCV%2FJCWH87FlDlc%2FhX0gPbALo8OoLqob5wWNSocAifUZ30QIswkhHHcdwEM9yfsyhWv9jHa4luM5hl6fqFQqakd0SuO%2BRx%2F7pN8vr%2FuUVHcX0FAF1OD7Xr0pCKk87144wQhLA8J1XIS%2BEg%2FNkmdkpaRyDb7j95uf%2BbipLslwP5ebtZUH%2BNedY0Qe%2FettTYD9lM25isgjY23PSDda%2BPwWtnkdRGvf9kCiabhOpJVsVVxIuqfcfSPc0wnOL0xgY6pgHzRikk9vpaajYs0yqTYqZFnzmz4HhWK32lAdd%2FT9sr3h7GqtCbBUhBelm9h1%2B8ohM%2BX%2BZAl7UuJhfKjnRAbpk9WL5cfbDVEMNlDxhIK1lJeXzUcVWqyb9qw88QSuAc2KA7ZQfSsWcIKjM2cIw%2FY6y3adykUSueNoGVG9brArrp2KRHQtn0VUpCFv9kr%2Fc1H7c5vYcArqi7dVIs47Tf%2BQoGJHMSFy48&X-Amz-Signature=a04b227489bc59ad6cceec35ab07aff1b90623caac5869cb03a64db5268f1c55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

