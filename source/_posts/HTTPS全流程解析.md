---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UV5R22JW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEjwjjjZFShYbcxqtNHF1fiC71KdlEmVtHtZ8t%2F04E3AIgKkp2RsNKHkNPa%2FuJVy%2FUBgrb2LHjCu0lTvpTvjouulEq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDE2JmzA8NPQQMGvj4yrcA9VLtkumXYd5pWQpt3wTaWaNO%2FoPFUNiVvtxaLPbfWSUO%2Bwm65PiXi1LkMxV889dqQY0w4naTPApVz4UpyynfqFGsR0bjPfyBHRyBjOzeh9VE%2BMsi4fVFtCnkx86AL1zaq2%2BUePZ%2BCACBl80mnrwjK9UR1Xnadx%2FRnyaaayO124we5M5iNF%2B6mJp%2B3aoU89kPtf7mBM3KOCiC0cp%2BRXkdTLE449KTdSBSmIve2724VY7GQqQs%2FxJ0XKN3mHEC6J3mvA6Hrxvj75PfwBcfxTNfQHiQgIL7z7ESEVcWu%2FWjEkXyMOundAnyCSIwdhwtrpK4M%2Bw5g9I4qi2GzQ8EnC1DkBNsYk7qv9hozjy2JGgoTaV1HUiQg1VEMCpu34dH82QvNrxgkEUwR%2Fz9Fc%2BdEpqZ%2Fw1h56SSFEvWzGM1i2DQWEHWEI5Ah3mqqjvQ4NOSRjNh%2FRmQ0yfanuC%2FRfycsm5WRC6zcRwaqCRFhj5BdVvQoXu64XoAbkTDCHzY1N9EClckxQc19j%2FG%2F2%2BHBajmgKBl5ijBN%2B6B3OjiAotJAc6ENmeapg2lugX4wSVXBfkw2D9WmjUK6t%2F6Hrlc%2BCLkjUJdJ%2B1A4nIMGsmQ1WSFyNemeBxGRJi8i1Rddou0ImOMM6JvMcGOqUBUelGcn%2B1wY8yryarkqv%2Fl%2FGPol9sJUtP3bOtdxjpaeLS0Bc%2F1VplE4CJLNLlIE0Lb666%2Bz%2B7OX0gTcMCZR4RWlv0mMZqpIcjq%2Fk6YQzUw0wN5V4MeGx7pa8st7QmihlTjj3MoxwXyZJIVaa2Q2qQ3uqi4k968Utwl8QNY0l3pplhrkRHYv10J5vbJcszVYeZwiDsh02zl4PsNHj2ugAmwtSJT1%2Fa&X-Amz-Signature=81eaf819256033b44771f03310b60d92189339105c8fd09e9e9813cd735d530f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

