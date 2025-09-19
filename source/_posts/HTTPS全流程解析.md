---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OADEX5X%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCkw96V5cg4YP1HqNzlCKyyrOttvDNqjOZe7LKalzimbgIhAKxV%2BKJ%2FMnz8tRzspIE5qxypQ6ah5uHFyF2yMe8mUlf0KogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2Fa8Z4qT8n5ufhgDAq3AM111Y%2FL2T%2BfJqjm9Moyjr1sLw891FPCaccvT%2BQCJ1FyMYwABF2fq%2FARhK%2BvPOqGnBckjTtVVMEk5wnumQPmv%2FjpFDQ3cTmxq9qaYRxzA3qVHX1EhQlgfUod2BethrTcxmgb7djiU0O%2BBTq8uu4tYZkZE2Sd0bo84KwHDJ16dOdd8sZQxhJVQWTAyUS0jXbyA9b3upCsatLBKm15s8sHBD3%2But4tIN93K9EBoUEBy1EZH0iKFX2Sx568PjB7TAL1oL3EVv2QpWX9KmNmzv%2BUwlg65vZwJvi9CVDzd03QD3Bxy6HSNTXtuFGiuxW3GPm6YCqzx%2BD%2BnEW3t3%2Bkr%2BiEeBLFsb0uP8zsGKc2gzk85yZi9D7jfyeuLwrYA6MLG5qOuplHhbjc8rkCogd38HxFqOpTWgoiXkaCniYcuBZ4P8XvEgQN7KUXukM8zUVz1lSND7xrOsFV31zYkmZXYMI5vQ2Lf%2FKyrnnUeuhYeUhvSqWh6a6F6ZDKvkEJ9Xw3fJzG%2FIMqJLEe19xtetgjWyEoz30rMaLw5uczyCX5ev%2B8jDluuCF%2FBBrYrCm92qQinVqtFgRn7FaJzgWDbOGMSD4d9QMy0rfaLXfrC5QwsEnwhhGzX1LrekvTDHv6%2F%2F%2BrzDSx7PGBjqkAeQmoj%2Fcrimbl8vW2YtLiDlN2JoikDUP3YPI5LR9ndaekCKmO73sEXKDIGAN137N2ib1J70C%2B7NFOy0IoaxII3UjDdC%2B6JtT5G4gruWglrKubpiwx0X5Rmpt07zjgfYWhVXb%2BNbxqmKNPxz8fefjP2B0DZ%2BT8yKMW6GsFl8mG7WlZj7m4jWAJvlfniePvQcWvo54wU5nLLQVTwNMiUvIKt00eaV5&X-Amz-Signature=537a5334ef09c2ad3cd1dc688e71c2824f6284a4d508f672dbff8d2f3767633e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

