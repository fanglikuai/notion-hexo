---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEC75CB3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGoAaE10S6AryU0VW3O2qGE34sedxXClT%2BkkCOGeiJftAiEA6ff5%2BGL0IWL%2BGmkIqpMZLS005qKpBb4XeDZf6BkrgHIq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGDbOxXZQmb1Fy6RRyrcAxrrq%2FCGFdwHx7%2BY9fQ%2F4h%2BdLygxRhOk1gD%2FpWUT6ooZfcVUWSLkC2btQoesOOGV316LyC6cNZRpxouyXrx2X2xtvj1uyUL%2BlC9LW2OP%2FqbOfuO9zpibqz5cmf7wZjyQSC7vO3l%2BHToRVrqn%2BJTqa1jBIEy9ClAnvEfwItHhbwZ4LaiPYk89HbuNh93ZirHez5Kso0kG%2BH7PW8m7Gdy9QfV5UjK5wCaJdZWCE%2FHvSlDrGFIzFBqrJy111JxI0f0vdt0yLobmdAarBWsIwrVarhT4qszApLEMPTa84RKpzeUWGo4VJqJZ3ymIDebPVn%2FlWJLuPnB0KT%2B2enlAc9fdOA3siRrRNZCnqxecAM9K7%2B6X8FkDfYmEqyVpM5rJWtY1O9n8pqkYMpxPOEAEHqCDAysLyjUKvquJ8rcyaxN6WhldxejxxP1V2gMdnZrjJSYqdBK5De8jnRAsI24ZaOw8oJNLZZpC%2BZIqs4NPinf80%2BvS4Wb%2F2mXD2%2BUl4RS7uAKrV7s3e1CHorfbCche%2BGjTcfY2mOGQsz%2FdHxHehTEZdOWovxQ0vfmpo%2F34Xg6OzvVMDpyomg6u%2F6Xi5wXcqgQpvcIqoHJBQXJijizvwHMvqoJd9l4Cv2GNAIhWTFogMM311sYGOqUB9mIAYbvl3gqZR3OUfjEkX%2FQ9hgW9S%2BLYFWBxxbZlicVmmRlQwVkJ7ViJPro7%2FnRogSCncap5tcNq3J07Rg8HBcLnhNED%2FYf4TMGL%2BVFJAtT%2BOZAux2AV8L4tmdsv%2BzWiLt2djoVoctf6nSu2NHgjaHD3wxqoW0%2BFUq8J9JFaCNmltx5P8k0gizdUQjwM0Lty2VPyy0GbSWPpbtEXdM19NVjUe28y&X-Amz-Signature=d2547636acacd1b564ef365ad380fc643f35d7ad9b92fa3943c40b18b71dfa43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

