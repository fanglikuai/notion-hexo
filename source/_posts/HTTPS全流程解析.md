---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2CC2HJE%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGCUk4mxHpp77kZBVoT0fgjwQdtRxffIz9fBhMKJQFeAiEAt4idbxng%2B1hl8yMUmvM64t8CZ2u%2FRWZENRouoyi2H6Uq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDH%2FANPpwinA8N2XPPyrcA2%2F4H0Hz59XCSjh97atvXNFsHE0ndEaQIDHVK2jqlEV%2BDmE4uPxD1fhCM7Ong0s%2FRIXrEgDpXuvJMcaWJi7F5qa7u%2BMKARUZf%2BoV8VqUU2CG8SErOH0Kiqzdvc4csRdsU7l%2Fec06KfmRLK4kNcEBvDjW1hPb%2FSBboJdDYbFYft26iQvx7%2BFMc60scMKiS3mLDwS1hcDBodgR7MKwahiLclwO%2FYdZG5W8earj6luCBdWoh1%2BqBKepSzQcgDtlUysKR0%2Blz4Ir7MqdZtbLKOTFnUTWw2lg8KU9bsjewbLKk2AQnqMqjrQ1bstwO6ZBi9EoJNg7o%2BoFj7AKAp0C7BHyIJlqxB9ELM339dRIW8xinbCWYNIMur53rk9%2BMHttOjKwKSXWtp1Vf0PoJmIw%2F879p1ftjPRgiTA5uBNABG80SlV%2FPS2vaNO2Lg0UbQRJB%2BPwZh3r6EeIBt7uh5G2Wz4Ou%2FAJCL1GfrEzxPcxppfyIa02qvrbo8G3dj%2BKNonCRSI94khahbdk2QX3H%2FzzW0nINEMplERSTj38HPkKrlIkjWMEvXH4hDgkuH9tHJ4eYG9xoPONe0fp%2BZ81o%2FDLZQBz2blFGzs0ifHpGLEVm5I47EO0SFydWMN4GdWDkOBAMOHPgccGOqUBBn%2FU1jJgYkdcWuzQethfm4ZrfI6jGHki4jdPZ6nD74Jw%2FHnfzLYCb%2FsfmfvO6lFF1IMJzTOscKEWVxl09gwxMiO0lGs9IGlSWW3AYae%2FCOYQa01sskc7ePEuZFc2lulMSO5CW2Ah3mysTDPZf18yoocfSsM9Yd2sDTK3iDuLRSs4EZvRHRWjCyf08GrKlw9jIwSB%2BvrY4t0l5tEor6GgVaQ7KcOC&X-Amz-Signature=13149139505b15969f07ab2fdba497a83fd4b405477410d1200784bdf7aef4eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

