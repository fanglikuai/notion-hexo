---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDUPIJBR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQDDo5JL7kzfVxAPSsAJZvAlRwUoJ0u1kSnlQqSlrcT1zgIhAIkGLXgVtnXyRzeLekmWvQ5TZef%2BFM0j0be6jfm2yBN0Kv8DCCwQABoMNjM3NDIzMTgzODA1IgxUAr2a52LV39fsJfcq3ANi4CHQBm2CGQ8IxyiWeh%2FeJE0WVTMei5quqSWmM30B52Cg2UqMDdE5GngEi7oE%2FIduiKGarBjdW0KCCASNIH%2BgUxQhB3bSpZ6bk%2FbP64dnkcxRCkhz2uYVTOBXNGEbquY1s0tl1yXMVTNMfAdgP9pr7idAxTcH%2FuEXpIsAc8eAwlJyUur70fidoyfIeofyiBdFz8iiHeApZoBCpqDx6fM4CjUdk4%2B7%2BQo71Cdl%2Br5a5wA5w447E7u2lkt04TiiihYwDkVdlGB%2FxahWcXOuHy9tt1JIxFOQuJjDXg9Zv1T7NcnNwWolBEDxlUtF%2Fg8eP6HBGvcDapeqJBcwgQu2jrAqh3xY1vbsRdFzyFky1Z4DB6GHRPk58V3o2rVXoia9NhVk2WSGcUDW8zBv0bAG2PUTe8wyRChHjSUf7PY0KZeu2giCf9UmxzN%2Bz0A2S5NFSyrfTJu9BTO6aucorDBMvxZiE1%2BFtmRMdydJ6JGjymAK3JsVVK2UVwjGEhbi9p34c%2BnwAdgBDSn472Tk4UGXdrZf2Aj8LZ%2FSE%2B%2FfoR6FPlAlNRui8uUHSAmKz9yD7RHRzGP7eHH7v%2BIAg4JQw81Rf%2FUg7%2FboarjHWevUCKSY40KcwzeRAZ9geWWyDvev1DDk%2FOLHBjqkAYCMCeTD7sf7eyoDEo%2FgsiF%2BYwrfWICGxVOgdpd5r3jKDG85UJ0LpFt8D0CUt332mO75qsPcwG%2FZVA%2FkivEWdUCVEjJ0tgG7FY1TbfWK8Z%2FCkDwuEIuhVFw5bNEJ53g1GR9kbEGHBr2Wtdk9k9PCzT%2FTwkkUGm4fUqX%2FJensMalzfrZdxvb8i5a4IJ04sjKX6iwDTifzi3rtiz58oMOZflyQJ%2FDQ&X-Amz-Signature=14a8813aa42cbdb95f04e26f1683a396cf944f7ed204d7a1d22c26f062d0dcea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

