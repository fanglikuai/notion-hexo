---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DYZZ3UE%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7Z174uWBP4VvT%2FoFQWRQX1ABlQtMJe5S54WIA6zdJhwIgBGFXVZ4qYPk%2FmPwhgNLhnYovPYWaKbb%2BqKcye1jCsr0q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDAQee9jv7hKNEglj%2BircAywGes%2BhKHcRXyLspR%2FIx2W3Vf8TlgB2vEYfY0MF0e0gj50GOVDZwiyCXxuHfxNgTiC9wfZSBlJcecdPk8Alu1AR2kWA9QuAiDnjGLkv4jPf3WFum2xWbmtkJkfcb01SbhsYKTbc8W6G3fetdwP0R2AAipBBML8m1XEnlol5ULYv40d7PAsrO%2BIHydEPQRMtkKU5RZAjVtAbdD1RC4o6GEfi9SvEuBJ9%2B2RsO5U%2BpDPFD0%2BlOBZo1A3c4E%2BgQBGKQtatBSjTD%2FhGqebkzKWM4Qjwi%2B5de6L5XDw3zt2LpBf6FA%2BQKRSg2JUd8iVx%2B%2BxtecM95DV1KIw2hZwHuzJ8%2Bth2YQumdcRd9fODiYll9um6rKXK8gKS3SNaSIjiGm2J8iApddplLGTybmE%2FHURdkLnb9EiZArxL8RhSYiJK1S9f9z9ALPxpbf8%2FSmWano9hMlE3s0ZtCp2BuxzLyDajY0ZCnY27ed709kYfnau3Et6zwkac9Mi6w5c1yMZR6aQo6grKpzENbu5LzptGK5tACA4ueh6SLuIaM4G8yxwIIw3Wx5vU%2F0HQi04aincpea3czuHylN1wDDdv92WsK2M6xpTyKqXxHkU3gpTlCzoAsrh2nSHAQl3HkUHCAqxXMMyxysYGOqUBMMKkvccFTEJUHiQ%2FjeETAU%2BlH%2FnNxPmZCc0mdY1qeqpaEjdsHmg4ZyV2UZ%2B5mFGDPxEQyfAnYkLQaN2TDy8Bz9whul7I5tkEuyI4anJd3LPhspis%2Bh9rpDsD4p%2F9%2FSYaJ0SNyeygAOarj1HKJUe9cWmfZe6%2FzcvyC1BvAswdYZTnHdkLnHgVvNpu8knbOVZ3vifNkKsoonLjBnhHnMGQzGXRe8tL&X-Amz-Signature=3ac769753b4655576cc90f5c5db30ce079557be36b2d04b8d9539ebf8ce837e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

