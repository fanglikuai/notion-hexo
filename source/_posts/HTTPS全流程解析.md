---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DXI353X%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIDOwldS7dI6soJEEVPLBsOOtK9RVzH79ENyQBhB4Z2%2BkAiEA7mWga0EDEnk3jf8ZZPjHY5hi9EiVFrO9wNyZ4HS0iUcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL2TS9mXyWQl9%2Fe6mircA%2Fn5ZRUHWq1D4zSurvtsRj4WZYzqrk%2FH67D9pC3R6mFBQzoLRCNdX47fKoGEoPJ5aznAByGKtG2zWBD1%2BsebJdMgNeSJ9vQhDDbh2m9xehlhWTdzmzqsx591TY3YVnFHXWWMBrxJOxNKYfklPHGFJ4LGRQzRvmx1axM3voaypFXHByl4RdZ%2BZk8u4y3whWOc4jwTi%2BnU4Y3W5%2F6wTmHMwuJtNMEXvHeW%2BUUmMCrMIGFk%2FmRhRIWpETBCRSokhekLgprKqFkUBV%2BDfWeLOXJMmftx%2BsEbmmKb%2BfswLNXNVgHf9v%2F1MRgbuqB8xBHnokYOgZB8WTiizAP0cbgVlF7vLp7eGTC2r6DxfbOif35bgQoWpdjgdVSxLcHmvshi%2FqU7hPzWQnV3EK%2BfccuuCM3uAGgTmFU5jTaLu9qyN%2FucnyQB19%2FSdnU4HxDJ%2F9fA58syA5nw3eTy5rg773z66ETQaDZyutYhe6G0BJM6TIferZN5OET1jKWpnbSoeRs3HR9janpqF4JlaXVa1LGcEvdr%2FJSdQ9k0vRQer%2FFulA3PVILDs%2FFf2ckOyLxSh1NVIKtoeVp7VceqxRtjLr2BhElFoP0FGqL%2FhC2pVS%2BGABV8DKhFLh3U37voh93z8rKJMKiNmMcGOqUBZyiAOU62%2FdvWJryKzcbTvMPHo%2Byink8JZNn2o%2F%2FKOZ90My7EfJtLtCOn926gk4Y08HRsR8wBh4wSXvuVlM09%2FXpGcwcWJv4eBzZUR8R%2B0h4m1hcBq8FrfW3qbEF2ew1Hhqqr9vnvE1e9gF9vbWhQLCWjswGbcO0lk%2Fu9KlIiQ0GwDH61aztS2rV3JYqFJpR1xeSIDdeCqt7%2BM5DFox%2BvrMigAJrW&X-Amz-Signature=efc8f817c87bb070134fa3118230033ec5a0630ce25d22085b7d68c1d32ab052&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

