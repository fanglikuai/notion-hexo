---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDHL7R33%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIHuagCeGVPJmkqJ92hEnHVli3LVGAjiKlyCAxnsRK3OYAiBVSX7l%2B9KkmRiSfaDlWcr8g0DC5zEWxwYkpwWSlOHH5Sr%2FAwggEAAaDDYzNzQyMzE4MzgwNSIMBxELaOmc1HkMjA1VKtwDJRh6RT2fRRBBF0sHBUEE1RdZly66aHwm8KLU2gOMNsmYOzlG1zepbNCHq7qTE5h5ACfxUvCBs2%2FLJ%2BE7gluOfkI4lmnVtfFfEGi9pb73k8SUHrnQZGvJGEKhUIoH8vTbVx3awr4Xnd%2F81m9sQ1OXhfmjdUo2D38ximW5xboYCECO3tDCP2hmyl0UM9mx3wmSS8uqdxpnaQ6KvI077iRIvaN1WVxMNB9sh4W7IUF1O%2BeSt8jRUz7zBKbhTMrxanhwuCXgXQkbkcKmc06V9Y3HNhdOFSz8YVgRtf7QvfM5Vlp0SOxj5R8xjKo2ap3oz3Is2VMD%2B8CFYX7cj%2FLls1EXflKA0S%2FIgztuefKeYmSRQu5idwQgszmJm%2Br8Dpbw6G2wnx7w6lQA53B0RZnlLPadwevfs3%2B0vF%2BO2%2BmzDjb%2BFP12T%2Fy829zfyiQLznR0pZ%2FQ43PeSawhkkvd8qp4FG3sJIUkVbEe2mR3qVW%2FSlpW0IaueCuFUo2vxISies4%2FSO750js854X%2BujJCCuNVm3yyRYNcCIXRiHJHNF032KY49aieEVKkis3K5JIw5p69SEK%2BqfRWLt8A%2BHxD1Siv0sxIqF%2BQlvE8jMog7s9ov1uW6RcaXcgmknyMNM%2BUmmkw16jNyAY6pgF68rHCAjAWpUjnn8z6YVx4a%2FGVhOuwS9jQlh3ws%2Bcmy6MdXBrwD9tPYv6f%2F3%2BtIiCB1wmvEzNQQVgMJEz%2Bd7t9g%2BsH1yI0Cf7kddBrDrMcxC17WIgx8mM7rwqw2aE8o46pM%2Fv%2BxxVN6tdcJZQ9fHFfJkSIM9Eqb7VkJ9VazdMyV6Q9KJDbJOBqPWIUO027fDn4BE0hFf4zqibS5vSJS4ZGe5z6zZO5&X-Amz-Signature=1d9b363c568a1e13df5fb91f37de7f8064a27cbedce8fbfdb74451e634f79657&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

