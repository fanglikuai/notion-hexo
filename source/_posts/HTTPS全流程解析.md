---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5VNICCG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDm5AoEbVmSY2p%2BLBCyuMRkagf1HjQn0A%2BRhiGnYvGyFwIhANVbtb2pOXmyjOePlPFU%2Fkp0o4urALOj3lAycaHH5riIKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwXR1yJXG8jf%2B3cjHwq3APmDdK06aYExddu1A05MGYFWWoCYsYGCXhE8UYrbkUsWmy8zYFLEOU%2FpFzrKTAB2vA%2FLlZFc4veUBSWNbro72vAmUGn%2FNQTCFBDfadyxUMGmPytcbrBXiqUshVYzAm%2FRHE%2F9b%2F4KcMq2fzhxa1U8huCNZ9mIHgrsDSuD3KteAzwu5yr%2FQJZt%2FJtqfKmjocbJG1mqwP9VXL6YY5NpDFb7cfx9xWEp9Ft2ytEX40F0f5EGKa%2BoohysYOkvdC2NBrqSFwlT8%2Bo6cXVtLyup700MYBH5JabOjr62%2Bn8R1TbbvujE1DJjIkA4GiOWhXkyk85dzP6EOE4ArOXy8k0TGvUTfmvaw1W4Jt3dAQMRkyMR27j9e5Pl6HjwPRPc69hhBmry4KouCheaU35Xm7xBXUpSoLLj%2F%2BOGVAqbQTGeEsjcjGFq50M7a6lZ5MshP%2FiErtoZgLzf4FUOmWTs36mob6gVoVoX1l05GM0%2F2FEe4HsUKeZliapFgq7Yv4iPV8rbz5LNHIdWh7Y9Ix7DLK7X6PQzWKki%2FOpb0ZoMyQ744rHFXitzF0n4wb%2FQf7J%2FG5lA996lZBHlfMI1r%2BQxXGm0hBwhK3tsIwY2RrXvXC97jj%2FXup3fD42OHyeGKnUqZOk8zD74qfHBjqkAfjTYRcl6T8eWlu9RdcqFjmoWa2yNrrrrTn1s9l3pafj0kZOYuCjaqJV9stwgx%2B%2FtFEUMO7KgkEvQuJN0RrA0aKOEoDi7i%2BjPbH8HHYt7mRND4q9BJ0TeLu0rRqCh38SgodaBp5%2BWp88SOWWFgB2%2BOpkCdCAiBH%2FI9mwaEYwVgEAFN46u%2BkCyHOiJAev6xHUuS2Ktcpn6nsLfT4eG89afQesjf%2BO&X-Amz-Signature=7b1dd3f2db302213e29daac5bc220a393f3c9e6edda7e4085863946497383fa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

