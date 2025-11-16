---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGXQ7MCD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAUX156tzQwJmL3lJQitOHepbRE%2B4HEHr2E%2FJZ%2Fe0b0%2FAiBHSbs%2FdNNjHG7Y9IXYiVnL3xhTx5q3ApdbZPlRqkhGkyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHyNCpJzM6mqZR1GsKtwDVa5BFK0AmPAQXYeQ4sENC2Dhl7DoxmLPZVyhhhYjMCr12dMMonh5YX10F77Hc%2Fn3hxVcurQLUfTsJoyBA%2Fa4%2ByQ3c8afVmuGYAyUkadf%2B3xKPRmeBhsstCk1ArDj3uoMgMiWx%2BShPjIh3nRE9%2FOwBaY5K4EYURzqLR5ZlrlpuYuceFS4y8ZGodHMLOORe%2FIhIFHCfAOLboi38CxX5eXgPdqEG3vY4FeOtpZSH7oeD0qyYiX4sfdTULi3szbat2HQ2qEMtPzhoo6a4LilN2a6W3qmz8d5eFMCS5HphXL7ewO0d0Do4vhneBMqEq4OfEF1Kgw%2F%2FOiCKd8CkyOO%2Bznsq%2BcidixuLVI8jCJgxGeePkTDCpwbxqIsX1YOVHMzwnDVruO5d%2FqdJUXa8y%2BgBxD1HzO6A88AkNrZqR5sgd5IulVmg7YkW3TMolcEjx4hVA2qy0SZmXEnmKXvUlfH33AdWdxUe%2FT%2BXORLMh0nIeY7Ii9yKv7Sxs2SvGPnmjUgiRuxxyLtz6RpPQDe%2Fr1tZ4Jau2i%2ByqtoPeaZxgcD9Ez52%2Bdd6pOfFh8qhi1uDK%2FWe8%2BS8Ebmsls3cDmnyKhOizfsrPR22syPTvmIDRy9uS5Lj2RPjwUtn6V8ETPkedkwpv7lyAY6pgFM7Q3VCNIUTFJD%2F3IZGeQy6Ueh9xS1%2BOTouyN3F1EqQHHeWvq0qcogh6A0fs%2BWC1CV1PV2JflKRJyR1V%2FfvsQS%2FbOiO8Oa5UMeyJcWbkNwO9JCV7dJgKCPhm5FB4eurk9CuZnmQkE1Rot%2BzVSQqvZQF8briXCguoFM0JpfHsofdnplsBWXzysvg%2F96rvJwZUyDyV%2FXm7YY4izmzVwCK46rs1JWJxlk&X-Amz-Signature=c7e4d038a664c0e5932517512dff653d8ef69375bcd8a091c52f09dcc76bd0d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

