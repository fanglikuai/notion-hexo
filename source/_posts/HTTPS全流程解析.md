---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRROBSQ2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDyUryK4gDc5ARNDj3udNHucW6%2F44JG0OXxCfmTQNTU1QIhALBRWfeGonFSHtNNBpz3HaJ%2FUzHFDD4jinNyso2s4o9AKv8DCH8QABoMNjM3NDIzMTgzODA1IgyTeaYkUmHWLSg96%2Fwq3ANQ%2FTRio5O%2BETj2lFAvce8oZz1nQ%2BMbiDcQJmbwcI2y0i30Wn7MKyNW9ZxVIS9hVb2Y46G%2B2AO3WOui3G41pZPrkEqIiLsmLFwligIdRb78cD%2BR96EXlFNJIUUA6ZBPORbX76uQVPw8VfAqoU0Lsvjf2mMKYJn10GstzMOInenrQ0EH3H4lpSdjR4r46uMWpP6XifbSIvT8E%2FCZ1aQm3KZrqcsqFgkUnT7jiTxZE0VNqCAEBKRqPJGXb7wnN852QvDdsy6u0zqrP1JXQsKJBA1y6h4Gg26cM92pVjtlAu9p43%2FVY4wGXm%2B45z0WAHqSivYz0luZk76XXY0GutzlhJB59Qv9Ez%2BRJMg4sVcJZlmGwpjqcqvq%2B8BMx2fB%2FOryA%2BiIxHQSAKspNpVr6MZDqUrw3iSEDQ84uM3DbIFOo4dF9IGi%2BeNXjIAY1bszO34Y8JmCHHzsv5p3zKmLWowo8GGdNGzKAUYcz7IsPdk3cOiDTCxo6aTAFFOOVvyeWWBlOem9qY0p%2FLi4D5dUIJX6pJZZXzv4cCPSmKWKY37zx7HsuFjvPArBXjEyYvC5VqGFP8K7z4zbwcA6YyvJm%2BVA4Xov11hv3Ksq68r56nkZmXZg0qF2Sh3sJ1ocjB7ejTDk56nIBjqkAbgSOHOCilpcpGNuL9spQ1j3ca1fHenboxcHm6y1wdZxukOFrI%2FW6fDHndFxe5Tm1UhZUKHY58vrp%2Fg7KVVe0yLbAQvXb9KIKU%2FiPc3OuncmZl6Eih%2FYqUh9NKpmhqQeEE9L24Rbv%2FrhAEslQQUBTnKPzXj2zCjUl28nFaGze81QbvODjAMDx0XzM4FOBdqNtLiO3r9VDG9M4e7TAayGxl0RJjeo&X-Amz-Signature=25161b8a24714326594b3dd9483b65b6b976556616144230f9ddef2b1d933927&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

