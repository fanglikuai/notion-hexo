---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5UTWZDI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQDPPvryBc3EKriKTBIJxVX4CzHXnG0eNBzwK2bFT6ZTDgIhALZ49kDWVVubzUl1aRckwKhahnwef5EQppj3ZBrRKxQ3KogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyuUr25SzAj%2FKsq%2BV4q3AMEHaxt6C26VkGQOg%2BmgKcRv%2FFE5LoSHU1MgaH0rqyqdcDjwKZit6nCnLGuH223Jz4XUTFRlYq5c2ER2Qao%2BXNHQhr09FFmzs1EjL19NQ6GAEc68URFUAPLbyvwZriPtkWMfKpF1rDpMKqtMtY3sRFO8%2BzWxTVk7TcT85OA4pEbBkBaCifWs3srWjvc8%2F3CYNJz46tq%2B96gHfF052KboU4t4KvNn%2FCG1zx0jtQO78x5Tgle4n4nDovzkKkVIjrKW3MzyLADfKZhfnuBFKDjK2hUKhHzjkqlrqKdqgEYbkdA3YhGJIDrYbSrFcbcgWS4P73o%2BsCLxuU9H46rxhvdZxfS815Knv33wqb0%2BylwY1FQlAvjWkubB58cu%2F5p6Nv%2BqMLX5nwX%2FWtrioMpbYBc9rOh%2FH6u4axD25suQ7qFUTIZYxPNuRfTId00tUXZ55MQ2nq2pwm11s0aluxf%2B%2Ffcm38tSAUsfbQFCaA7Cc6QNxsGeTsRxOb2j5T0XGAywb8lRoDllIU28%2BYW9Y5QQS4SK9DU5u6Z78f0J8U4vRn1pNhTVJ0WtadZiFjkLRmi47IBo1MD%2F2ZRd366zRXB8D%2B0lmILAaqI%2FBB7qWlL3k%2FeiJ23l2CmF%2FMgvHpMTFANKzDf2Y3IBjqkAdOIZorkZdte2CzK6iTDr75yEFnVKi%2Bcd6g1lWm3Xtrf7bY1KnCypHJj9V6KgDrmYo7UhziVfdA9c0nK%2BXfwVToGTk2uVIPlnVXtINgEkjYcHyIhSZpWBRowqA9cT2XjroApnoPixFngyz7o0bUcB52UU17s2t8sSt%2BXUTBVVI2YIoRZc2eHdD%2FSxNVl2x%2Bq3Zv1PZrBuL6ZbsKAzmMhO3svO9xw&X-Amz-Signature=1ee019f3ff1b857f7109740a6563d5cbeb26bed328780c873a9213acebc4f7fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

