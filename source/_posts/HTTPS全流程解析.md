---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QET55U76%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIAP9YHmjPrM0RSYqtrQbvfEzUgbvHXiufTEi%2BK1rSNxLAiEArtBCrtRlWYWY%2FZf0x9%2F2QYaYYsiYlkdnm8gWBy9AowEq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDEPr6CEaZZm1pu0zdSrcA9x2IZRLEHjYRELp%2FyV6Ax0tJxFbISG5LgWABcNzj1zbd%2FaBJTmR1a64BeB5r1STCPavx%2FotYMKSGJMtl%2F1xEGc8RpBNa39xlHgDMIvjDL%2BHpuVsv5uSD2RW1IT1rsHJe0KqyjLyJMo2tyg0Ry6be4iC7QEa6iT9WLGByEgWkCyvzwQoQdpFH8hzIoTI8lGeqpPihsWaUcxqRvnc6EXGM9Aieidv2FXyGxWlBju4GYdrkOQWJt%2F9foFjA2XCiZ4i%2Fvy546C%2F%2FrDDJMDotXTaINT5KwHj9alFWk8cdfQtc9xLQomZoUFErPEs44pF%2FujskRFqNJG3lA0OTndi5fxkMEvbuNu2wkv7xbmzHkUNU%2FI95tJ7ZtWoSIIk9Ts2Yx%2B%2FRWxiJF%2BoouDH9h05ONWh2n6q1A1rVVOogBJqMmL9EwquK7OJTB61FvHmrH76TVfi3MUdllXU6Doe4Y5asMVGAHF47t%2FNR4hEJG0%2By%2FR1vJSASr926J4ICXmAsKoB97sNqudEtHgSmoe6Vmq3jCdiv7JWRS%2FBh7DDdmR3Xk%2F0wZUi6kk6eIkMy%2F1favH4w17rm2WvZpGXUb9Fl8Bt9oahsRbM%2FYYmqyWXpGkx6i7YiWMWdifC6vThNfcYX9%2BHMK2b0MgGOqUBm5meHvdXPEQFeHYUz8cIB8PGn7nD7%2F7AQuhmSl0uhCjs1Nu0WVQ2q1ghypjT3cqHqAZqLsX0aUlYrZPUt8nUuzHUOcAT2QmIgI3Q7%2BDdar1%2FLRjf61Ebwleh720uuGVf9pTJ1bwjy4fiRKkAfSPVJttruHOZ6HkZVfztQ6%2BR3WXW1ijOCPzhUniBgGuEP0qau7J5L7Go%2FFl0muXBOmYNOe3cmBd6&X-Amz-Signature=4a9d871561a4bee1951bafdc58b2d9fa8af5a578346e1ea11ba0527de0128110&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

