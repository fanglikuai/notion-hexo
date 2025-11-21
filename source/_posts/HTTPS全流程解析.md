---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPI3PRL3%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDC%2B14XTYx1me5CmllXDTZugsIwTZwl6vFNynld9mzdSQIgEpzivkhaQaS1PB3qXOItX9I%2F3nr1HRbJ%2Fk3GU0nf2Wcq%2FwMIAxAAGgw2Mzc0MjMxODM4MDUiDKbTMTG1njIHBL9JtyrcAxQCuqiKp5HGEfvRVNBXzeKhliR080QuZLty8aHg81ws3aBU0On1L47y6heoZ3qO4qCCbNb9doJGpNpu7A1ZT8XQNMvKE2Y0MmaKxSu%2BUzR2L0kSzlFv7KzseQZmSChfTOFJtJ7OPVGiik4cn%2BDsQfhw5m5gAig15eTMe9wTYxIqveAy%2FlSXMi128W0MPow3yQPDP5nI%2FMwkJ2wqH%2BMVpQU6lWzQva9vwWz6zJlEalNZC9fGPuJYbQ3XSQfuNKQ2okwy%2Bb%2BuH%2B%2FcePipYMvIDHapm%2Bz3o6u7bAFjPa7Q%2FoHBRr237r5tmx%2BoIRF5ctAjuz8tcwPAqm4GiaurG8vSH33PqnBSehr%2FRw70GJhxH6rSANb5zmFiTSdKH8hEp21JB%2FUyTZWcWa7cLiqK0tf8R%2B8dDjBbPgoO3Oh5DtMePSm2GafD4fmO6vs9SuyKx8lT4LxkAcJ5D5LJ%2FwLEA4orf3CVqL4MCslBA9YvTqTrm8zHNi%2FbOevsGtdTd3QloGwHE6S3P105X3k3Yn7fo9QKdSnkJgWLA6q%2FMv6Op4eIDc%2BrN4cAczhadhheWKYhQaDTJ1k6x6%2B0I%2Bh50%2FbjgXTFYuttpaxaAsDxNYH9uFClT0taGvdMkBVA1iqk%2FqV3MOyL%2F8gGOqUBpzXOmVakJwMaRAM5l8qSjGB1HG3JKvRig%2BomCo1W4bvuQMFNABFbCFQnGYdZmwfyqDdpw6ZAI9hzBPq9s5d5FHDNpPemiH5AlmjguKhuGt3CE4IKsoSLikkzuUpg5xGa1KqvbI4LlBSpJSDhcFe7%2F%2F6u9ZYhxKyI%2B8irMBeY8n2Ts2jiNuXDuTa0%2FEJ%2BgLaYuMQLjblWcd7%2Fd0D8ydFMLi1lz3NN&X-Amz-Signature=bcb7d3cc1df02895a8c4203b402ebdfdd989657f36c56653e4ce8be52df5ffb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

