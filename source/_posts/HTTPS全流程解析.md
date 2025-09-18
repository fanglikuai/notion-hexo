---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JQXDEOO%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T020112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIC3dULDok5tjhth5%2BhU5xArbwRBkkT7XF2c9yKD%2BKSAxAiA7ZtLD89cMsjLvG1ujybXytekcUlgaEpbH2%2B4FF%2F4e9iqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO1QDqcrwthBlGAw6KtwDigy5W4Cr6yea5twcCJIS%2B4BH9scSJl65xgTm8V%2FGNF1AH0kiDeGgpuCPPgFbW6XyoFrPy%2BY%2FtW1UzD3JzZ0BfW3pfE%2BXLB0MkFUghxh0x%2Fp2weyyzOOQn4%2FZYA4B1jZN%2B%2BIVznsNojAi%2B8zECOzXNsPGbeWiSM3qtEa0bO3Vai%2FXcpo5Fy3T2g8xPHp7KjWv6F7NshEYPeueYRi%2BxdhSHqmYTFy%2FHOYHr5tpwJyc9sw57qpnglJUgbisOXhqRq%2BUAP7GceET0tNz0UojmJfDHbIOLe7lADM14x0hM1KSy5tA7nqLzt5yrtHQcJ5VCSON0V6pcaeoaSF2touX7w2t0MQZLC1GG4dxa3X3wYoz34JlNclfZMfYjn2egxZBwLPXjPX18Frnhz8GpsRrhmWvsDqViXpkwk7u%2BNFhTup9WP%2Bi1kmqMo%2FuopEbzyqMwkB4m5qK6e6DFTKiv4CP5FBD9xsrDLUOdP3VCbKGs3AP7%2FhD%2FGNl3RLQFcT1jOdRZ5mjlu7G5VPfrpiugN9HqSPenOYDuVjY2O6QqVoYU1hjYqiwQczbwBp0cFA8TkTQUOYKAqhuDQywesw5eK8iHAzdPKNfFiAYM8ih%2BqUPh%2BQS24r798XitcDK%2Ff%2FUy5kwxZitxgY6pgGirZtOL4igrEU0gfqMGddwYXgD8N1ByrHVtdFo0rks22vVcW6MEk5rrPuL1csAZBtvQAWlpGqTJyIwMfy1tbmkjqBcGlDSEt37x%2B3FRlYJbX9%2BVo4hMS1q6RidaEhdhhK7ON1nrW2Y6Y8D%2BCDQpVcaqj54oza2NQ4sueEUHAyxyiPnMj0CvEsbu1XUMhYw7TOJJ6d1Ebb55fxJDOu0WgQpEhHHoGXL&X-Amz-Signature=7f547f4f7d150184a98b1e9e15ef1c1579ed6e8b8f0187935c6be6976faa9af1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

