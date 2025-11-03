---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE4U4CMQ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDvQ6ka6yvl105BOnldrbRX88coK0Kr6NJZznoEMrcWcAIhAIuzfPkg1H0QH17F44I4t9G5dW%2F26kyfr5mNJeI6G0xGKv8DCGQQABoMNjM3NDIzMTgzODA1IgwgF9H43GUIkBy4ba4q3AMxQaUlnE2WFM6wTs7HykZYR7W6%2B%2BC7TtiiTB70SjqmESQozqDMZ8kw1JpXAsGayVlO68mtwyVGR6muZciiVvSXOH9ArhbIWhg3dU%2FyXjZvmSZPmsk%2BAmgUHaU9oEsg%2Bs1QC4lRNMQixzISP4XHv2%2BJSRSR%2FSHoF%2BP6gtmDjnwPnkhTqKsKs%2F49c%2FrqFVDnz2WR2sTT6JGvYpDx02Ig2Q%2BHtzMY9hic14NwPYsPhY1T%2BVyeUMzng9S30bp8g%2B%2FBmFHg54uQPNilGYxi9Ti38BOfGVl3e2mhoWUchLj333vgGFzX%2FbqkVSEZM%2F3QrQuUPPSW89%2FL0TqH7jH4deZNKGFWQNmu27QF0mPN3rQnpNaLyVNmAF1sl5W%2FICR6XjwFIgFhBdDm%2BB254LLuw%2FETszXRwoEKTHe0oSYkR2Ieim8VaL%2F%2FnXcWZAQmAU5FPxD16shjG2VqqJbPJKsFicOKRv%2Bq2dJxF9cLJnLNF0en8WPGhgUhG78TyxIGQcNOIC5hf8DHF83T7csBzeOiy5HFRM8fZws1q%2FSR90N5vaOd3jJIe2grYrxGagEkC6BSu1XvWBJwB%2BmfOT7gilCjFZpCqAcecfmvHZWSt3Gepa1HPP%2BnMMANhrskcJkNJaJDLDDt86PIBjqkAXA7vm2OY0ds5OMgcwWsbNgdBIXQinx0IylCvbGc3HHv65OFT4EEXnPohT7c9ubx630zjjk89fjn2%2BLI%2BpcVq7%2FKzHGzfVi66cn3tSd66F2eGErbrsWOiufSFFchSx0x8moAxWoJJH9mONIWkZd%2Bli1NghtjhH9PbsFc1h7oZOjVeqb13s6B2Wsl0A1xToVOzr66YJgEhyG3dX5bNK0eJRbwBuN0&X-Amz-Signature=4544a2c620bc09e60cb80d73855324f9e26bb49956ebbc9e746bd18b16e85a3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

