---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FCQKAN%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCICP7RqtV7qj957v0qZ0GxreuEP06NLJoKHjFbh3q39NKAiEAlRnTJoVUM%2BNUzZ5XdEfQ3ozLeZYceoLBAVBPtOgJCDsqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF9bPm63QkTpKoQWJSrcA1n3UoyC2UMGdK6ABFqieVolAWHNDWsqDMEGns8%2FKdzmms1b9JkjAWbP2G1emPg8MoSUuV7K1e0qYTTObvoB8YOb3WvQ5W89FjUZJU%2FD3hJ0meondfOiQT4wuIKohDZsU8JO2BRaiK7HK48pG8%2FSSCF%2F7aHaWVWJS07YPfiRdru4Q1jUakMXv6bcg9W6KO7Bpc953V8EikBvcdDh4mJ%2FO8PVYKO1p%2FnJhIvh7r8LWsDMlSlCT4XyEg9DJeD6C5OncoMkXe8SVj97ZTRQjc5VEar4E1A1VTncjRCK6m12VF0YlmuQUiDRc0YOoqp2IrzAijGJdVchic4ppCQcHIXpXS%2FaxxxC7YF2cuJFinkekUyaNQbVXRw2J%2Fs4sAV9P5kuhHykLxeY%2F9GsxXPB3Cdoc%2BszK4uQr8UKjItZ2%2FtgaYD1%2B0O8A%2FeCKovL2lUCkZLI40wcGeSe%2Bgv84pgh%2F8MlXYF9%2BGeE3sMUDb81Usuc3Q3AGkeBOoyC2TCD0VHqA7QAUPVbEYZ8CdqpCqjfhL0%2BkawFB%2BIKyWmJ0j5xat41hUbaw4cY94VaAFdTFd9LopQLRFKw%2BCMOwZ%2FRx3hlyh9PiH28JyXlYB6POrc3WJ%2BstFH%2BpcE6c5ivGZVPBCmmMPmYpsYGOqUBliJ7CMAwCSLaKilqp248mWIfPAXBj9pISkYlphCrWuoUJKsEg01WM9ef9QPDuOWt5Mmaz63WLAVBJyupcK8CiW97Zs9lvJ4wI1O2BF%2BxfELlaYPoC6wGGSBzbQSVGmuG6aWPR2yTP2cq2P0SD%2FawiAewyCdmnXPpIEbXBLL7cnOsqczcONrQCXwLG2XGsFnnd7oAI06tL5yXZxbDLffmIDoY5%2FdT&X-Amz-Signature=3ae73a774f0404d383cfbe8360623018e685dbca3f3fff0d5c06a3dec3fc27c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

