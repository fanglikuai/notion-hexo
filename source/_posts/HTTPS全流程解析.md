---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGUVRUII%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQDKLBHdjKW5i91rzFW6icn5%2B26uSm6GocrookY1Pf2p9AIhALLd4hcCjSzFTMwOz5HVUqt4FL2e4HQAPB1g2gpLCAtGKv8DCBsQABoMNjM3NDIzMTgzODA1Igw1m0fC2PrqrXgSOUYq3ANGcSgrOUyfR1Y72TJeHOQKQSCAbV9QtcMMziIab%2BFEwQVrJQlUvYy1nWLOghPqxzCOumaFUYs2ENzM0X9yNk7jIcXZ3YZaQS%2BsskyWyfki%2B2Yn6ARUsMli8e6q8X9Izf7mQd2ktK56B%2Fs%2BV7JLMVBJDqKZI5t0ayCtdzt7JW%2FcykbUcXGR%2Bjehasdv4hrwH1va7XHwMPhIxhg%2B%2B4EdcKJE8fwYudIniHCx9xPzzptZE2lG%2F9PhBEmNiMCQbLCnRNdR0b%2BwJfbjZqgHCWg3uqq3QvOo0yx8yXugd21m35KTPf%2B%2BaEBe8gl7TJZ9mOJGzqUu9qngD7nmEy0DUSkoiKXjjmUsNlbpRuIm0Ms3PqRBbWjB8R1FUADcv8fuzzovK0sKAqbjcFlE%2F1RSZN7g2oVwcCs8b9D05kk3AP8OxalmGNZp7rcTORKlvu1xqqxsu%2FTk3SiCEf3STYtqebucJHEqHfCt5kcZGiqQL%2FaUfCf5PzCz2OdXGSYqc6YW3vlL%2B%2FWjcYrILvmikh6i4sdhdS72Yb%2BxBPyhWy5gja%2BflTgffAjGJyPwI3I9R%2BKcgZu9BhhdkGRD%2Ff3ugSlZiPZbJgM1V%2BuYx5pVFaRZr%2Fo8OiBhgQs2GYx38ToTlzJnkzDRk9%2FHBjqkAYeiEqBQhR5qVYLuGbJiy6u%2B%2FfUwHds4bC%2F521ot3VpkxZdTI8Zko8bUImn2uq9Ar0ALCqWbCKSHsR1IdsHzveAcA%2F2s0C4Sdy4lXKdyhb48hSlhtT3izZYyQN21BKzjbLNvVZTBOTiECo7eYsGcapBb7P5dalMJOvAibbB2DG3nrHl2%2Bv6EKfgobBKvrF%2BUUBngbPKOGR78Epa5pzmX7y9fpemv&X-Amz-Signature=7c7322b47a1ab7e931102f0df4bd0592bea370df21e625143f763a206eca921f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

