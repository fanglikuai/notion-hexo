---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYB7VECC%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDq2z4cWVqQ6y%2Bzk%2B83Xw3aZHZaRVHnijbBdYBxoTXLcgIhAOrWi6zd3FFY2Hf7Jyo8GSjxc6E1ScQMuTvFcUXUPf5UKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp7CeMdKLXo9kizfUq3AOeS5YF1oinqU%2BZtLs33kNBmPhFfdUkwZJ7MRGppPasYOIVVkr41seqyHmgyZlQAbDikOw7T0floWKrfZypsUVZNVT0aqqUQsvMq9UiV4FNRXmdNtBTRFTUa6Z51bmAfc7%2BaVFSeYQduQH7gL4nD39qKVAWYwkMzlVH3UN2RnlToyerRjMF4nWu7m2FgZfx3TiLxt8YBwWSTLVbAZy%2F%2BPJzRLt0rGkGE6iuiAvEvW1MS2Z7U3M7YxS%2BzE%2Bm8AbzN2px0iGDDvUiLX2Yk8xFjb5P3sHmt7%2Fx1jBbJ%2BnY%2BIqOXpIKDsCjIcyek5Uamnq8tC8IUI3t%2F1afbW5NTC9wibC3XLpmBy3IIyiagem35Ug%2BLyg32koSryzG3SOHSEOIs%2Fke8LNqH2Vd5Fsiiu750K2%2BFcevTTK83OMG0PWn%2FXTzyPmPfT1gphEM2E7qFUPL%2FWpwA2DbqYKpENnAGOGtKYd0Y2LnO5JyCYburfpUd1RRM%2FEshQrzd9TDL%2FCYn2cn4mEPfL0O2StijSgMRa3kFI3I43m3OaA4hE2N%2FMttBrTkTdXi%2FBNs%2BJz4KG5LLacZQ%2FJzwdOICzSA0oaTdgeBUuuoHi4fR6ZKGAXSwcUA%2Fgpytk7IMo%2FOayzDlMfbhDChhYbIBjqkAbNzxn%2BGo7dJYHI5vOXdj6gpx7bbYhjtTf2SkQMrZnj7ESwvAXECcuAtoPnmsabaRof7gCggNZFiEcRFd%2B5KxMAq%2BihzThSpg7mDqZR4CE8bC78GVCpK%2FUrwidUvbmL73O8I2PMHBcM%2BPw40ampJ0TOoEk%2FoJ3J3eUx%2B7YMSyEcy745LWOrD2ARQZl%2BsOdlfYKymrzCf3cT5HurlhZgd0EEvVNKI&X-Amz-Signature=e6b5a9282e756a368c8b9ea816e4f11bdffce8df25e5c571fdf0395ef7b57e50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

