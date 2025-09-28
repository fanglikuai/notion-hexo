---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRSLPEE5%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJGMEQCIA4GKiGcrBW%2B2mXLrVq3SLY2cEjRF%2FC1%2F9xvznFunyyuAiBRu4s9vB0ETCdt8w97dzNb1MEGeVITVSsBNDA9i2N5CyqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXiR66kXR9Ld1pxz3KtwDGMAHKkinEYBpiZnadwoEFRurHSpENBIyxK9EUFr3BKISHEd3THaiYZSjGYLsesA7wW6%2FwoEI3qL8ceL7nsCeFVbrDKXymuOx%2B9mWOgUBBiWLJTjx3o3teN2cnSLW8aPepbXFc4F2EbfLDmJxoWz62Jq%2Fu0C9xr7hxLOswkLGmKOsQO8XWK2YjwEhQ6NBMni4w4TJw5pNcsuUR4nM%2BDln2ZVFH7w5uqD7mVA9ic6LL5At7TNmXKjZQ60JFdnGrKs%2BaOYvzdGnxCltHXrWkX5ESgZLDbFGQEWIDy0a9DWrvlLCyQwjzGhE%2BHmqSPW5IjqQXnbKd5nYCOkdJ19wMzMmI%2BaIcUO%2FBkdNS3Z%2B7auIsboUeqTVB0NTz4AykV69mZrINlWa56xffPMARN3BQYxMWOmaS5p73dEeqYY4yTOrhjExNsjdB74fl6zrTvcEsqzzdEBnrkIUc8FiN3FRNgC4Wlmscqs%2BQn7I3%2B4arX%2FWBDHD2oGMLUgZGxyWv%2FhoTGcH3gaJ0ugqC3cK0547yAtB1bKnsNL%2FxZk5EdMQkupcoFekdM5KxF5%2FLuwBWLSI6rXnegQHSM79OS%2B0su%2BYaa7DPN7kKKEuCHTtGVQcktk%2F1mmZqpQcIL4ZSy2irKIwv%2B%2FkxgY6pgH80Ius6RhiX1MbIsJq801dZO4ROitn2edOZ%2F57u55jmYxYtwXPMmDiQF4Snas%2BiL3fvwtsGfFyMzy1symZP4%2BmBh84lHwwTtQNiERcO6e%2BmYZXouHOcCYaRAEEP0RSZzOXZZHI2ISReSupMJwSETlGjhVP%2FLFAEGFxsJGIWq10U8TW5yXZUtwbBfYZS%2FIP32xmlooMRq10mjQIsDg3oHfZ5nWZXgPy&X-Amz-Signature=ce65d7e2c3e8b6f30eb14725df29099f6be523edadeb04b1c7f9d14a8296d239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

