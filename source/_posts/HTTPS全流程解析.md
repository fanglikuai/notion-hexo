---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRBURS2X%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChsxTjNpzikR4Iw9gNoRx1UdBdGlmz1Y70HiPT3St%2FJgIgOMpoOYyB5C%2FXMKDVW6Pux%2B6dGlYscIAEJcNg9mkrt60qiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJhUmmRATyB5ZkmfYSrcA%2FvzVxZ94NcrmVjqs6htn84t7EPnIeYlPSC9294Uk%2BGFYJgf61UvRqSV7an09IYulH3oCQwpIYCSDuHzKL1xqttspaCeJWatOAI9xtIYgS9B9zV88DxrKrMcHflV2AHkNAeFNthqugGsM%2BDUn5qZZDQcBMFYjjJPI9cqtfN4CKKPy%2B1FZHvIoAbX5Yib%2BN9o5DoENy2oMQDnPwLHj3QLXYN9sXqdKQfkI83D4q1h%2Fnk3GFamu1NXt7ELoZ0UD%2BS3c7MK%2BLM4paYiy8yweKviKk09ayM2kLNQTtqxACJ7Vy34AQ16XcYBQzC86FwmrhYtCJ%2BhVqyn%2BFGYp8ke58JTQ0zW%2FksfOA49vu8JtY09rjarq3ZaUgR7Tb5w2MuQW777oqFeSMUmXTM9GNO8npxOZJ7CMKp2YTXvf2Nvf%2BTk922%2B3zYSHixXgFE5YjoelrUsNzJrh7r86ic7xlJkmO4OxxwsnFBGl%2Fui%2B68GRowzrNEUqx5%2FiJMX%2FxwmNvwD9dbabqR7ruFeVI7pDwvG6XNEniX15XGMSFFl6BYV8WichybzPP2LguTfZvw7lBHTJQtvkIHhBZZFkA2SMXVha5mGaC2kJ35ZWPi%2FcOlcAkXTK7vRrVMQCDW0T6r5Zee4MMX%2Fi8cGOqUBapsrN4N%2FaA0XNKLO7%2BlwSJUPyCbfl8purteK3%2Fx7RGT3AOtgaVNFjOf7qsaUtgwIJ5%2BIe60Q1aQ9tRZn8N1gNFUYaF8syc9iIV2gJoGaBM8SF6106Z0dg1ghFNHSJFDqdtMjTphye4IZo7c5LQSAUD3ej8I0awlaV78LnxhG3IACXp8yxPjOwmV6XRi7pexKZvnRDKC5v3aCajJJLXFEqXzbr2Vk&X-Amz-Signature=502882e2c322463ba4994dac1cde2b2c153279666c85c0848266ee8f2cee3fcb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

