---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STX5JA5P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T020054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQC3tuZ0%2FuO5DnkD%2FSV3LoLJX8oGtmnlJf74ot0ROM9gIhAJuwcXfaNQYMVVV6jPOFjBzJqtldYDiNmjoB7z9ms%2BJ4Kv8DCFIQABoMNjM3NDIzMTgzODA1Igxdww8WbheFBhBLZC0q3APwMDKd2vRWxmvlYcMBXemgJRUwctPcTrFl%2BgnxrhTRD2QAhOsqU9XzutIgJkI71tsbDKlEZ%2BjiQV2uwdigXGRZ54fUPJjURHK50ylMCIjc9lLkl7Oh7W4WqevHHJMDEiw0Itu4oZmclfcQOWRmsqIdvGeuJlRRseVAWe30WervZgMILQkimZ0GnJ1FwgLCz44jAPREfjc8QqYfH1L4r9eBquQ8VYubo6TGd7mKx5QEnPN0rbx%2BHLSN%2Bq88dHQ6paRXPqYOHhXWQodyL5FO22c3FyZG6b48qhLgd0OMw0Rzu89P4WiWshVi5GYFSae1683LZgtVGz3JgVCzh5FnlszVw%2B7GnPts8Q2c50U4%2Bf%2FMHHJj19XX9q9ffbkeU0z89MEAYpCMmk0NG391bhJCk9YG42LyRJyGHfJB7Ed4vaLnVhlQc%2BoaSWbjje9Rqcrc7zPwnfnnhbKKkNX99ej1I45LdeWRDyqKVfZAx3NGSWW6W4gy%2BcjjcBCjE7R%2BO0pC5qpo0YVdRsd3uWXXBRum%2FrEBm6%2BKo2%2F5H68RYAg5thbDTurS7n2A3Zv0y0db4w2ahnYBesTBdU0yUs1p4ZYX4baiCTNEK54%2BVSz%2BXB47jl%2F6z0cd6jRlgqzP9ThsZzDhqevHBjqkAcUXNZg3mD5lgasy94xeFbIMP8P22OLHw2A4oM1dpdiITbJdILMZWg30uuQr57eDTg3rjM3wAednv0xvJodwaLiNuPsfYuGwiRJ5WhS9kg1kwUENKm4kg4OqzOwSUyEAAOmAQS%2B3mSe%2FqOOCkbBclSLYhcTI5d9fLW%2Bp9pXkJ6XmwR7fveX%2B2WqLOqm11hWqrVETYc%2B29nWIY%2F%2FsTNirIU%2BXdBuC&X-Amz-Signature=810806767cfa1458536f676d50eaeac898337fffea93fd40efd454f0ca564b56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

