---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X4OEJVWD%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW3KgvFCQrzyM4LSplE9J00zkl6g4Z%2Fok1hzFPhSBG0wIhAMvklf7qGjYrnxxnERzBJC3C%2FHQBRP4oBe0YbUFaCZdSKv8DCFkQABoMNjM3NDIzMTgzODA1IgywFlEyVgHMNL7pDkcq3ANr%2FhE0FZyU%2BLU4xt2%2BrkEdo9QGY58HLkN5gTwVVCS2j3aZiUNabZiVe%2FRkrg7eNSusSQBl4wB2PVPnHmrUL%2F0nJIKuyT%2B3wwNjYi92DuAGYV3dIH%2Frt2clmcL6tXASkMOhpxiY%2BjYKtes%2FGoWFAN5B%2Bmhqpeq4iXJ3eBeuxGDF9gU5Y%2FODFhsuTz8OLjYzpCgCY%2FOjRB7ng9ZUPfY8IfA%2BsFIrswf7uHtswkCnngGqKGPpXGpB3QuT26YASo6VFSxzLHyQbAl9sVoGboyZQZvyJ6ZEV7TY4m0voWtbeF0lDPBUIULLt2gZxCAdyVHIGOUKBHqGnsCC6pcMI3o7WhrcktaCQ3PZusKuHd4ZuuldEn4s6ZdaNwq98KsKKMMSXbma4dq6y7JfIJol8SirYnZS%2BulLkbgxf3WC%2FIdAXtUXCB6jDn6yp7Kpd6rhyaDTu9KTBWpUyFjQZ6dsmmOpLiWVriOtc%2FbuMp7bujVW6TmyTge7YJS%2B4cV7esjjyKLVRYPI5s5xfVmrZNYQ3%2B1ia%2BHRXN%2BViPSnZd5fnSMD05vGjkVmaK3s8mE5GwXVlxDfNlumZ1Y2IQKs%2BQh4mS7t%2Fbu6uUxfme3X9rWsdtaiVL6Etm77liLQxbaPZawk3DCJ8OzHBjqkAQ9l3jHxMvbT4FHjK%2FrkNaz57Zec2NgyTL0AqhElW5Wy6Ex5JGWFDepuwgA%2BuquZ7ZMCVrTJY4FQLbKntxD4SJxaky5Q7IRG2tFNuLDxI4wDHC5OyPVJ%2Fo9GDvEmwHTgkWViISYekEOwGPFyEE1zTFiWIKsbaKbY8PGydB0aiRH2ep%2By6vDG86BhLH1PJh0ByAuXkWbiU6Ud7PNJEcLq7JvT97rI&X-Amz-Signature=c610fe6c906ff5b68817a686966500377553b0ca915d60d48647a58b3e1c1c16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

