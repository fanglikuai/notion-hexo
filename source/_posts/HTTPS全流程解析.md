---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V6GVIXI%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDEA1TRigjg%2BhaBMK0KqW8uuCzYTf9erfLSSXVLF1jRGQIhANF98dC%2BrZ7PAF42OlUV437b8FfvLGXs%2Bx23tKivWW8wKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhGjDCrdIWk7LCQwYq3AOYNPy78L8ZWbtKr2l5z1Q%2Fc83arNjujeBMoZ6iOnr63CE2C67Hp0wm4bqakwKENIOG42b9NLgfHnrKgonKCW74NbZMOfUMIat5VHUxAs1uMM%2BM5vJ9Qi1xVDi1AidIHy%2BDnJhfdtOP9TPMvZbOkkP9tBmqlJnZk1zktqdYuDLIS%2FPa7Wo2WE50w0YVn9bJ6oMgXNzijFXX3twttB7IVRA4rY1FeNPYvJFeETCeyxflUteXgoUmNUaGm%2Fs%2BdeETbYDba3IHsLOo8qpvx1hb5g4NDebhG17ZpOH3wJ%2FU9HqYRr702IB%2FMo1P8%2FP9XAD7x8DuNYm9nrVxKdQ%2Bhr5D%2BGpOtnlsgb9jDO0Je47wyJbUSHKOCKvWcpjMtEdjzu54Fnj7NQkfnuOvKv0NHJ8Nyh5B0sxOs%2Fbkq88DlSfxDRY93r8EFPq3dzxacTF2MkjnYIJKEIlXUwOQt9Vubfhf47CD%2FB%2FToBZ2xuBIyGqwA76y3KUKYg8jCV715EGkcZSCuPvXtgiJBFycgUaRuJGHCgQhehQT6gY%2FPqa69OwjCnJ9O7joGCyuDfnDhM5Bp8etb6Tc9l7Zq0gSvPTNBonhCusumK4YqCM7sAbbOVzolKQ%2Bhjz3p4pN4RvGI6XruTD33cfHBjqkAR5Ot74r0138C3YYn4qrXsnRreqgFNTWqM6xpNfauRpD1GZUjC1s02oR08iUOmONkT%2F325nk2dX32riTJjjKPZJ3ggo1Xp5nx%2F5w%2FBhrGsQIvUpENRKv8JxDqKGRMGLPnfuu%2FDjaNQktdnv9krIgYaEAxgw%2BsE3otD7WszQZc2hJaGfCsmuz9pBXEtJQkam5%2B88HlZ%2BhOzHvCYNNLRpDrGS2td7q&X-Amz-Signature=989aab73756a1f8ebda7107b129425ddbd660574016dc5b9661187d452822c65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

