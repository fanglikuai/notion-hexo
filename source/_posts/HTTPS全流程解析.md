---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JK3W3D7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJIMEYCIQDxZYrjGzkuN%2FANjhhH5lv2LwJt%2FdKUwlEqGvP1sV54NAIhALsV8NGKGK6P29uT1K%2BGE0hAYp0weIpA5wv5%2BZTDKtNLKv8DCDIQABoMNjM3NDIzMTgzODA1Igxzo%2FMQ3yLNHjwObwMq3APC9vrvA1L3vNaCHUAu1NxO8QIQTIIet6%2Bb4jHryrZXpMFEr90i785eFZgMEF%2FLjtSrrpdyFG6x0sGVrjmRl5ujd3sNiMcFFivM%2B%2BW4sIHmSKvrmVXY22Qn27JgniVkEvqSpl7DkKe8pVee%2FGlEDxc4LMggkVBeTy%2FevqAst9Y6c9A68nbd9Pc2JqMaPhUnCIr6aVr5r9BrHYNagyrlA7xcJDHJUXEPiJAdqZzIs%2BXocurX2H4emBCWv5NbJD1tLqtrlzhIFMF6tuqF0vvKbXaSPgpKtaUZFASJdFv6vz7djv0qtlsHceKtxPdKgdgmFu6fJKXFMzdhF2zt6BHjCeVxAjL%2FJpezrnXp%2FYI6w%2FPf6D07P%2FYAjP5oUpE4BhSShXEDUaPTMIm8aHOHdZ5vpi%2B6XUUIpjqLduqWCgx9VhO2c9vUYz%2BQ7VRH%2FprPqY%2FByKUHFoxuwKREHgy9yGyoKmmKRgTzFqyl4FURZHPslpvlgJGezW5%2BoZOjN%2FiPFQIX3Ba6RQECKesFG2k060S%2BoctVf1iXJFrJyTs9a0yv9S5iJHcQ5KTunzK7siV5EYhPZmE7xxlxzWgn4agj2Q7X%2FV6LTtzobAXZHBUxI%2F5GtvsIZFE49f%2BuzVP6IrDxmzDileTHBjqkAQSZ3bLcZ9P1HUnKwk1%2BfOh9GUi7wBoySiAQE5uicc2RuGcqu41DEnBMLTUx9AhiDtLzO5G%2FYVc7zCEwHd5jpscG7GFvmbVYEtpqEM%2FewfuJ9Uo3id9PD0f%2FyTw6QgOqV0QCEtL8QPQUsuRjSCkRcQ0JlBytpyM0sStvhUCq0ifW5Ha6nwyft1A2QuYLjTqG%2FS4FdSEegfCj4BuNtiBb5hWyWLdT&X-Amz-Signature=bb887855771c3710789d3af65b811057fd7a203b099ba6e0cfc281c5a2358c6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

