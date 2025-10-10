---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSDNNWZD%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIG4q1BT3AGVqLsGPQcnTDXXpJ5DiH4uEXi82sdbTnZMKAiAhKVB5H9DES83FW7u4HSdh%2B3uKt%2F%2BT%2BmRZmQib3XGJoiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7Nlyg1akrzgS3358KtwDXCRn6DtvQkOcM%2BnshhBK429eCsuuSPX0GmUakEp0jyO00LXLJB6f%2BW5E%2BKWhelLYfa7var7xDwXaaMBqzfKu%2BfEb%2F4oVmQ9%2F%2FVekfhjf9oQ8n12Vyy2X15GLiIHtqSZ13Mecz5Odl%2FIJ10HxChs9cZ9MwDhgcU1CQFBP%2BbJbPuuorddgoh%2F%2FHzHvbBY5j8q8TEnubLi67kDpFsT4DkRaJGuiVffl2AIl7Hv%2FxmWYCw7XdwEWB7bYZ7WN%2BGH0PlCPy2pTDSSJHBKLgb1rYX%2F7fsuI5n9A3ybVcRDvR3bO88YVBB6vBu83oPe%2FvEBL97H%2B%2ByjvncYrKNjGz0731rSBG1BlwKY2MsUdNkN3mcrnKkdEZNQhQu2V04QOJRr9tcpHZ5wQjtIG6mVN60P7Zn3wyOum0uQbFkGvCiExK3BdP2B9MvX2ugk4FF6criysHCS0ZN%2Bnc0%2BwH8itrP191XannK%2BV5e2l2oNrApK0aoBoefoPO8uu8l1bSNGFBmW%2Fx7PU0cGGRX%2F1aD2etwPmEO8NXADAARkwfVK0wEYtEuVD7KOZGpdYdvZLR2kGtIvhirrypmYdiQHr2glKu72ddNuVO3fVHTojMrk7yWR6MYXZDClYk7zN0Bd0YSdrrDUwqdqixwY6pgEqTuApL9B6KV51RJIWzwsHMOjkErtgAG%2F9gIVMO9byujdo7piEGwyxgyJxIQ%2F4aAPYD8lSb06PG96SfWATyyO9ky8YWTfbngHe%2FZZti0pElQT3UNWoBp4mnSqGqbniaXz8%2FrsAfSxTcRUiaxE5AeVdmfZdV6GeKavlC%2F4FK1luWcXhJ5FS6rz9uvFGf6X1uv8ABV%2BjohLDl6Zz36gHgpwMgWRy1eL3&X-Amz-Signature=da29dc5182a21f59b99016271589c01014e46e9246429335558b1f4b0e6691bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

