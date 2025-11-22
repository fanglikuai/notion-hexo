---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFQZEKPM%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIGjm8Wa7I5ek%2BmxoCK1cmxF1HHH%2FRRTFihnRqf4eNkWwAiEApm5eNzUCLqn5YLDQcuGuwCLrwPqZ5OHGSyB6z7OzqfQq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDEv0pPasOzjH1w4qHSrcA6rRfjTKRheifnBSdgUW2XNRAT8uKBVpy%2Bz2GQVCx66%2FBfilSFzFBd0gXiFL3ERlS03PzWelq0mCtiTMXDikk4PKMLch1vjeR%2F%2FrGbBUgqPfGCXsbWf2wiE0oHG9OmWx%2BwmCDnSfnAudjOJL%2BtItLBn3Qrin1piVvXQnQK7bHck0UyzAC5wbduqutgrieUGUu6Quve5CRJHHwsPPbFbTSUsEsYsF2u%2BeCp%2FJ0BHpjuL%2Bz0ZTo%2B1EfJc9hlI%2FNmTc%2FyCZq6qE7GZ8Sl9%2BJjEoPj1XLYfu3eLrs1Vtu9typjNJTsyWPc6J4i5Yyh4pAmZZKb8A3%2BJ9mFgUCKSw0BBurAg%2Fgp6jOQFFp3GA1wnnv%2FNPVdJibS%2FWxg4NOr1sGz7KG%2BIv9Ezeg7b%2FmGlDxAV5ucvCbT6TH0J7yXD2yW%2Fb7M8%2BGSKVcYzfCQ9Ro1obAjMIClopUWpw21qamp%2FYbSiF1r5TCOCBVcpptjcJjkJapu4w7BVHsa9MyNOMyxFy1UZxGOXgjNslJqTeN1CX6TU42Sk09PEzYiGrqVTanECxNEobtLiTSFCHMMsSf0%2Bct9V7jS6pilndv0q1Vhkq2ppWY%2FcHRdYpOG79u%2FgACzvYpS23S0WLtQF9IHfHzcIzMN%2Ftg8kGOqUB9gDxE%2BTkB3m1XJGQTIMd%2FBkvmj%2FTYK2JPC7tUCxbS8ZR1%2BXiJqahKwFxffB4uYUs3cz2y1yELVzkr4lg5Jc4JHBZHian7%2BMGY6RixdHJO51B4xSWpAu2ZO5o%2BopTthPCEugc3JbGY4bhOHaLTcAp%2B1X6Wtvm1IvPzZL2%2FyMGpm6zQdnlL7Cz5cxYlvO2%2Fp685S%2FlDC13HglDcQddONDj%2FMJsc53f&X-Amz-Signature=28cdc2f602d534942b8b4c5ca9665f0147b9f2b43f052345a2bc331ac99cb8bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

