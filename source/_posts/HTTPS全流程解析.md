---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626QYNZXB%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIH342%2BmMHkHQ%2FNO7k0bU3uKDIx4%2BiSpVLDvLIc5HxD14AiBTSeE510OIvgC7wIOGBbmN2bRbEt8eCVf6riMgw1LgGiqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXdGiKvO4IEgO0sHbKtwDNr5eOzJUn4gj1FAlP03yaeHVS8yodHT66e%2FlEx6CUxkc8jU72ZDQ9tG5Oa7P%2FRYGhaYFKXXx6cL5vbWD4s%2BiKNG8aAEJo3mREjrdfiDYcRtT51srQteGPG2nW5Wu2miez8Pauh2af9hAFTotOVdW0gN6Jh45PTriVP8168dpYeC0psWJ0oT6iK1BHQwbOI%2FNAq0rmvwnOJdFnoAWdQtAf%2BRFoE%2F3a%2B5GrXhCC3rVpdQodactjr8OE6X5JELiAdXOUdrWj2sv3HYBmyCV0URT1Vnv7ojT2kJjEjObIqaN%2FedaGrjxtmFOJzCL5JYgZHB3wJcVdql%2BsPk4eYUQ8nWuHGjE4J2yNaagTCgjwPwZClafJsWxXPqbg0nup1NqnUYNVg16yyJvFAyw8yA%2FcDRvN7tvTxcMu5vcFUu3beN2qJzm8BR%2FXpxZpZdQKVDa8fjHsMWOCImnj562i5hO5JGod1JX9Ew1I9%2FpELrn9ckg57cS6B%2FVFgX6xpgvE1GQ%2FegiiHcpLXut8pfm08Tfonks0PEJVSwvNfUB9vP9TNli6jYzn10VIVnO%2FSz7xszTP5fqmfK3rpoIOwDZNUj%2Bdvgaf75Nboa%2F7Koa6L7mom6pEJENReXuMPu77E2O67Yw1urxxgY6pgGLnHyCaVAortU0Hl8rvfaTJ99jivoy6GofaDAlbmZr3kv4JXDGjBc4UNuofECZDGT4nYwoxgNpsjAgPu%2Bo66Nuy8zfqfH68apqRALh1m6BgSrKpk6FkqL0%2B0RPFqgurAhTmI7cvw%2Bocrzhfz4lUpfQtfUZywONr%2Fg3ObFWOES%2BQCJsA5ULA5rx%2BcbNRTRO2rviFgL2ERzN2LnE6MSqhKO1vNZf5cXW&X-Amz-Signature=539f182c6b50bdbec4c6e4150f90b2fa20957e3abf68d7090c68d25e48b3f89b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

