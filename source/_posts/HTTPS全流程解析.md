---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PKKCW7C%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGOob%2BJto1HWuYxxBYyLwI09xrsTZapFfBCe8FDvMYoyAiAPC2wC1vpYjhkutiCf1sicYI%2B0rA6ytsHGXviRzCLlTyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMSzAkMc9XPv8U41KtwDmfJwn4jkiI4UuQo1Q%2FPCaoeC3MvDV%2F%2FY6M7RLL5GVop%2FaNeLRh7uieCAwudAFMjB%2BblBoUlI6fZhYKpsYipPX%2BEdPK%2FkdDz5AsBqV4n%2FG85aldHHc%2B2ksqEVf8PpazRENEiaXrTd344uf27J6uMN%2Fm9hWUYBvB2L3tKZeVdwlC7pykD7vf0Bp2mbstsUvZ6v4szis49sVdF65E0yRmMJYJkB1qSYyBxj%2Bw4JpbZ%2FUcqSlrwj3yKIyOWwHx0WSvAHjgWpeko2%2FiGppliO68KjCAmvKTcHB5uY%2Bn15r8nvIwt3ivEAMw26QfcXT1QJLM4VBy6Hr48X8pACQCDZKVzw1tpw48r2H8idQoFbZjV9%2BfwvrMoD%2FO0io%2FN8eWJDW6oKn2mEmpDRYx9gFRUwCneCnJFLOpnK8Dbr364b16QsBf17SXxB6bIergC2KbNngiFGdRyBcg2y%2B4%2Bs1UAxBpa%2FPa7frmK%2Fdz7Vx7UZQrRNFQKKY%2BBkIEn%2F2eRvCg8A48md%2Fmtr91ToW0RAN%2F9cNMt3xZyB0VjO0KaNbI22lZuLvnHqr5zC4rJGyQPD2OMMzkgZOZmxHZgsjDDo2Lr4OH71CVQrHwfINzTiXznBqnQWIxwZvB%2FfU1NiCy2oElQw67%2FkxgY6pgFPaonAmPNue0YL6rsRdeMVdiK2G7jstvkY0CJvGriu4TpqWu1ne%2FJjmMiB9ZgwI1ZAd%2FZ9Zu2F6KoWV9HZogJUq5Mz5J4dA1peDT%2BRVeI8D7SWzVu583YYHyUJTpwPPZDAKpg1blo%2FVgK3gQrGIXrDkdmz0udOGQoSvS90y3b0OuCnIWI0uEW07BjNALRptxtSkqdpOgIpseslIjw%2FCFQFI8t1gg74&X-Amz-Signature=f2152c7d67a83f8e92e5c79d9028ff4e3d1c2c0868f9bbe815ede9898f259436&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

