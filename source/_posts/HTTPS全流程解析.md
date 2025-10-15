---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665764J6DM%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiapl4dVjIgfu7dvqJ27HfsFVOIQjVmdmyxiE2JiBclwIhALWek8%2BYsQtebeS1OXmOmboa8EqQsWFXtPRt0cLnlYOxKv8DCH8QABoMNjM3NDIzMTgzODA1Igyxy2bXKPMrt8Q5N6Yq3APP4tBgEgk2wHAO75GhsVBT9F8YG%2B82fmwXEAYTi%2BWiOMH18DWff9J7EmCJCBAD2gzNR0GzZsAAk%2BkftInuO8tAlleTQRZvmGXxh93QKs6kevJFGFDrKCALsvnkg%2BNN6wQk%2B%2FJE382BTBrYdAEm%2F0uRK3njFA%2F5Qlru4ig2qeZ9hk544TOuCgKMxfj%2FhE5KEbvgMVhHUgmCv7RDkRKm8nMtbpiCh9GHEN1A6nXxen6UmIkGhGtMbXL3hu%2BCZNbQU0GYjL%2F9j0DOQtHtNKry7PO15TH%2BRdeD2ISR6R%2F%2FkSiJvXXdfp5tB%2FaL6eCztp9XkglJS7GDJXKQeFL0LBMIrcgku7BQcWzET4%2BYTRgX0HLKQdvGgqROVOY6YszjwYXkKL7wmvc%2BKZCfa5pAkegslPz8zeYoxddiHEbmfXF5LIprhNXTvW5Ybf%2F6%2FePcPNgnHNpnGcauA54nqrsx17zUMm%2FihJ7yD9sOdKDWH3GO9Z9dAqQzSm8%2BfQFV0x9XLemhfJVo70IlmpmFp8U1qNk6WglVi%2FcFZL09JGT9daOlOJcOhu%2FsWCvQjjK37qYUDsZ9HtrX4yYiaJMGG3ckRN3ygE5nQJCWXBQgNQ818DRnyTZWNcUvGs5lyJrCeEESkTCetMDHBjqkAd2ohZf3YHI5AhwILAvC7vmE7PbYgTlhMWW9T%2BDUIkkw8GP6bKqeJ%2FJT9G7KFXYMgfrom9OhwSa1KQoIzQBkTJUKHE4NyObdlVepauxky6ey8eOlhfl7H96Ytt9zOBfvQAaa2quEeEZBWQPflkGcnzkLe5nGHdHyRWqDwOKE4cgCziS%2FcDl7ZWbGxoAUC9UjGJJgYJ9kgUEB6ARrq%2BaWwsDOcYcl&X-Amz-Signature=aede7e2951b40882964883257851210f99a6354e492f76b7a0b264d54e19280a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

