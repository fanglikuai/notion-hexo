---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JQ6V3Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCu%2FfQXvtkmZSpVYNVYYieACJiux1AGM%2BQkwMIS5ujOBwIhALPvP3I020sEUXxgy25L77PJDKsiXt9UqYI%2B3C9a0iKKKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxujie8dxHRIKYFuPYq3APIn8Qm%2FVyX4sWqSmAxu49QP%2F8bUWcWcegsaouFzGOMXLRKksof3YDB0gbegDNRJkmYlIi54Mj7hrIG09dFziQk%2B7IE%2B1VXNf01IXF1sWlfaLjnhxUpi4iqepsSwsBkNacJP0%2F81ECOicxaOp%2Fkn0EU%2B4Fed8lhkj%2FgoLMLsED3JoF6n5HEJ2mGEdjqPYRbIlZP2%2FlHAvMTAJocHXExh1gEn02TUCUt21%2FrliP7wOYWuFBqvGu4cWVa0Xdx6TKM77PlhNlbpdyVKcHK7vjzzy0Mb1%2FOXynW1XTa9NJtk%2FoTKbjTVeIhJD6LxdBNE%2FXj4uDbAvQPVij%2FCIb4W5RXfo2rQqblKUUrrGiNKYo6q%2FDQRTFNw7VYtUU7Ys79qBKdr9t9EFkFIUU0I%2B7siwOtc8bwt1H7vuSLp8TYec%2F0vNxxG5IhrW%2BnVnBZr37cgVcLNRK7e9gISJfiuaGEONZ6pfY12UclzJVIPTOX%2FFVfQRdLhuDL4ENrpimFSW2oFsD8vlHR3N480zA5dMtfvquRy6tXqmCvIBzWZ974io7YcI6Raq2Ojzq5X1zp86wj4ONNDtV2A6se7UU0PCWmbnlHKRQtRgNWw6%2Bso%2F4vfN6U8uAnJpv%2B1u4hkmRnQ3FURDCJuvjIBjqkAY%2B9fY%2Fca4k0LPlCNOCzAMY3qkQgT91xOpzG5xkcnysitMCpcj3%2BbmwTDplc4yi06AqUoqtD4Rea2j36ljPGffOt6RnZr46Mn%2BUZzkSscIq3igQ%2BZeSisvovgxHY8m0tLqgC%2BaiuCxUhh6kPThJtIlwkH210QuGPfyKg8V%2FWILZo4bYChr8US4YknCxTfX1%2Be5kOYRtdJ2vJE4n1jR8Qv5MzlrGj&X-Amz-Signature=094a89d097058440f449a4559f629d3b7f57478506d40ae1908d29a57e133d57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

