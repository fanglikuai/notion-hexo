---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVAJBQHJ%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T125107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRRESIxO7PQJFnAEgCj9jaBiN5kb5p0Fm4%2BXIr8Xyv8gIhAKcnIFF6iAqseeUtOLoz0KyY5QajkY8aQqfj4P7%2FlxYzKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzkfEyD04%2B3fnm3qEAq3AMOlSUsYuV8UD7gOU6XQfL3ilkdvcr1U4O0BewG8RTUqf2kp%2BrM9sZbAv0ISMuYexNtZRJi7Ffh8YLyyIZs%2F3JWgLmO307XZlt2DGmfAV3wDcIYsn3ymy427sW1fwdUGzUU%2Fi9VSw3vTb%2FAi8aypbwJp7uKOSaxp1mDgjr18QA9ASKpXYgPMfIY2u2VZvhh6%2BvdQsGAXDazQs5jH1VBjxPyLqo4dugTELNJpz8E1ecFK1XLRnUz8sE4Ink6nYWBtthsMHPYX7%2BsOjJuoZgLQ9IjEZGfGtoCKS83mooH30TfQJobLB110h%2Bv0yxjikJKanY0eI%2BupnpMIMBMpOo1ldRbZKlVA1TgAQbV1GhklZ7gwaKiSkfHN6RBVrNHsG%2BmHVOjgrnWAGqlJYalapMxo%2Fwp81JaUv6OohIkQNSIx0Ggb1xspI9oIQmigUcnnsvP%2FdedJF3yo89egC5xj%2BHrOUB4QbwH0IdCAG2mam8S6IgU3QNEqtp6NexkC2lf3IR33aVZ9Vy5LiDNQ4OwYV6uog9CAcBsXCmpImo290c78UubJlZLmxnqlD33QysCi%2F45B96Td%2BB1%2FeuG4GHszqqDKvnoJF2jXXkK%2B6qMbCKjolj29Uq%2BldcJKcM%2FZ7%2F9WzCG8onOBjqkAWsVoK1AnJaLPCxmXmHVt49JUF9aUdgFguxK1s19CipEE9nQn2t0uKWIZJsSEm3wHUEBvUNm%2F3zGVZzJ%2FsIB8mpt1weB4qwRCzIP%2BacFtO%2Fk7v%2Fbx%2FJXku%2FMoa4IVo5P7bD1cZYRFNTAZz6%2Ff6D0fLG6xWcgXK8b%2F5wAhoCxUPYnmiNueH%2BNn%2FfMjYZ3Mh%2B7e4mhsxG9AzN7l9ROJ32h0NQCUGfr&X-Amz-Signature=41c39b1c069ec984445e79b2735e4d9fc3ae111deb3c53893c90d16fd8bacec1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

