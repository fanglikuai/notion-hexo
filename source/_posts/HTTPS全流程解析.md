---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663A6EZZ5X%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIHSnYFv8oWDSAZubxKEW%2BC77B4XQ7%2BwhDqP04Kk8hu%2FjAiAjLXzMYhQc04KAAZIiJWejeUTn5IstqVG6xcR2llzaYyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAtWa1GmIZN%2BkVxW9KtwDF5N9Gk6NAn0VfeV7sYoUYT1QAwS1DUOLHkokSzCFQuYh3bAq9e7%2B%2F%2F54X0BGouqeEMgWaROFZcLTHed0zTaWC99Yg27khXCO42Tm0PvLmpKx32NleHw67twOZwYs8sBVRYmfNONke6SSHy7gMh2PkEcNW4MQ5%2B%2FKMN4i06TDo7u5yZTRdq1lqTkaG7GJ4s83lnnfpLelSviPnlQHJaBaQPAeua5g0rfjEdUNswWo20ED6UB%2FaGrn5xen9k3zxQEdZkQXyn3liK12r1wSrgR4AMAS3FKavVH8WIvyt%2B7n0v7n%2F4hvi%2FXCZX7u4I0471J4686c9PPTofM3DCZjtIPNBuHlJVsMG4F2zaKneZ8BtG14G1pHL%2FPJh7LrAakrlWOIhv9YM4vvZ3L%2F2tOvSHVu1eGOjcIpQ%2BRYeX75S%2BY9yxVuNFIdS6vUXbnwXJfRc%2B0bItqXVTFWT2a5KXETVIZoAGDVqdI9A1kfgwgns0wj5tvUWIuPmCCm6pVUt%2FM2QkhEWGY8SOLg36s%2BNGpOeh1TVy8uUKKCxtNDPnQV5ULQZuNi2HClDh6WUjRQw%2FTmRtektnAYPMu%2BKAfv4w3xRG7DEECaWE6qnic3QvaU9jhxa7Nh%2Fmpo9d1jD2rtpAAwu6vnxgY6pgFQVfj65y0wie09xCkfzL9whyqaaftpORp8xoEbfBUXfZVZKoeHl%2BkiYir9V2MGqEmJ%2B5uyb9%2Fz99fyUNOUYrGOlXVDu1C1J7AAcrgnNWo7N%2FNsHwdMob3yjbvZ4qZ3PIqlvUwxH9X9RF6mlQuH4CGhtmwVcatTGL1L%2Bs3YoYDMuPba2nzulMu5i%2FRI%2FKtE6WftHEelOAmsO4bdXzSpjAzyOr0qm%2Bvi&X-Amz-Signature=f4788140094f9df385bd6fbcb6f6e20935261bf2724f7ab621231089259d47fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

