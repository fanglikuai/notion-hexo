---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NSBJOQI%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIGK2Z1mMAvgTwnPQR59O%2Btx0%2BrPPlFoqiTT394y6THWzAiB%2BXIjqOoI%2BkZtwLAtaH13ASA8gYtvzKAv7YJ5GGQ6J0ir%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMpwmocaTB0uLSGB6EKtwDGxucYPZLMmBqKyUyjK4t2%2FOwqJFjxTphkaT7GR2wKa9yoLBPrlbph1rCdjdrqfC0zXyHUo0Bj1lW7z8Xlmvoy0o4S%2FP3BaRr1h01zlLPMoeYFxFfXvwuHnlbLKAFT4xARKfeUxJO7bImRp4IpsgoyblmI3jMw13mAs%2BMqu3H6mFvcYsYTW6Fuz2dZtLlMo2JGyHF0XePXTufR5z%2Bf%2FL1CjveTqqNmrt1ElO7mIsk9cFBsrzBn9sdDqvH7I4a7Wyvej1iaM8KHLSZCPVS9vnZ6I1rkUvpmYNpfGa2nvBxatswWrf5VpJ0iCVqXgIrDc%2F9I%2BS3mrPkxP06bNANWLQ%2FBnGvo0c1ct5aSDpAy26tcJQAoG4VFki2amxqNF8lriECVbADti4%2BM21W4wUpr0l2JEPj9igKYd8yA3xMWM4YjvBsl6vAMC7%2BUS6RbMz66AENlH0MFOCzkLLMYIdNbzVUb68FkmdWFg0lr1wHKxowg4hZifRA9%2FA5NID6OvNPJEY1jmXkrb33RyO8XsUsLUMzQCjhKHcB6LX9SR%2BLeP5a8IeCIIJQuX%2FNQG5ex5DIfAIRa8m%2FmOUuui9yUmPNz47s%2B25SMFFtVifFnMfGM0CI%2F0qLsFGhaxvMZyfu3e4wndT%2FyAY6pgF5vztDSx5e5CANht9vnb3JFEhXT6%2BZJOJkKnsnu5vwfLURnwdjdCNG1aQlglnRNJX9BhhpZGg356bFszPbI8G%2FL0M3ZwZFl3HrzOPHonuA2JTkLg1KyUoCcxIsakm6Gs1zsc0F%2BfDHYR0ajcCYI3WtK9nhVyxFBrI3a%2FdA6GMjgJDi8MrWAjC%2FZtZVq%2FIavNLbhlgE7AtkzCrzmQ7465%2B8CapzFE2y&X-Amz-Signature=6692ef0242428fc1237b193e15729975fc71dca489456546252eb1dcd7f928c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

