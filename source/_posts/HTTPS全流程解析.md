---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RHRLAX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIEUuZilhdogSr6D1mVZlKJe1KEf%2BJKuxaJIFDuQu1fMuAiB5G%2Fsasfcny%2BTvAsdJ4jS1BUbdwesm%2Fk1dkb1SOUvr4CqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPQnYaev9BdhW8D7hKtwDcjvvAiZDiyYbAp2zyxQ921jU6LylnXrxhSoz2o4ZyPxLaEN4x8polasjF4xcCryfsq7ndRYDjjz0oyTBIAn1EQZ3SDHUjbW4JJZplU9dDf0%2B8HY1WHvOHrr7l6BTE%2FV%2FPuH8xPFB02egzu2AMQb47DGtI1SOfbJeJCoROXXYuKE%2FkmcQIG%2F9TuhXzuTzvkJXtE78aJKDgfKiJ1VE8SnAZBmDjCfmu9MvCcZff%2Btd2h%2BX9ZaxTPzuGi3U%2BhRwz%2FwE1TQuSbTWhEw48HAH7%2Bk%2BlcqDGNiQ6Z0twQ3H%2Fwo3wXQZMGAtv8xQjOSD5sBWgq%2BdZQ3KBlUuFVjWTRrAoZP9YEfll7nnq1f02YYRqSs5ssyYdF0aXaaXf8aNWD0ZevXi%2F3OS8BuHeskZSp0GYZp5tO%2BW1EM3urfAJazLveL%2FGpwtNNjYgmBy15i9kl0AyOLHSYkpgjTlpXlbqsVNHU07LI0PEdYRAxBrKyniaAaLNM%2F2%2F6%2BVr7HT1H0YJMLQtcPBHu%2F3Rn%2F%2F56cEjL6xWyA6aeKN0PYBA0BHMAGZRh91DqdGuxZkQSuGWcMAStxl8g45xW3beAU7pZmJBi1S2JWZkWH%2FwowIGTQfwvBQF3QX3xxUFw795J0lYSg6YHAwmdfUxwY6pgH5jayAeBrVEGSvn541Lmdl%2BN%2BWtAbPZDu0S%2FsBwXIIfVA1pHsPQ2NF3pmYBZaYUpS%2F2ZLnycZAK8d0Af0mE7G5C5jIchi%2FYw6JQ7OdJfpGZZPpD8wjCSTJvUDTOJWQp5pO2dg%2Bktw7dl9jJcS0j4b5hoYvRr9imTYXlO20AySpM1LjmInJN2g8uNYZ60QUl11f49E5y8K%2BGbppYOQk22p%2FXczzmea7&X-Amz-Signature=ad7b42d9f8cbe649d7265dd306d873898b7d1301dbe3a6ea16e2d3090aa4fb4d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

