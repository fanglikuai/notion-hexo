---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VK472J2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDnVAxjzQtxe%2Fj%2FJZtFD7U1ncu3ppI3yDvh2p%2FfdTbqkwIhAJ%2F8ZH6eBtYQnyPQJxihUdwB3jhQ2Ikyg99pr3LwrCrIKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwAIatjgLrr%2FmBh0UIq3APlk16DeLqvRUkux63l3EujFbhO4ZsSr5KPWFy6BSB2z0L41H721h2cyMZfJw66glHz2tq0aKHKpS2Q7DKEbLkjrbenKpVtPGzhd5TTVu3CX5T6B6ne7xn4XnmUtRPiI859nd6hXIz7mF%2BdFNE2MZWzn8mAR32ywerhufCkHAmiYVwjSGoXbLsflogKEDaXSPidMIl2bMFKzhHj9hf14KxwM1rLC3QeAXNCMPY2rOPBYeNPxfK2pTKGMoX6x1cPH3l2gLfxi8bP4vn%2FXuQBtrOJzSywuN%2FGRUm13Vfz16Xlrb0YSnbB3F4GfuBrAiNsw0r%2FuE6XYoSSnDu34isOyvbi5F%2FX%2Bh9vT8Oz2P9wFrmyNvfBf4YJFl5ydq6%2B3RP8YcHQdZm0i%2FaNDjEkJr6nzM1eDPrMiVrqi68ePUxatH1sOVKgyk3iACD3lNk1PzB2d120Infg29rtQ91y%2BwdmXaqKRAbd5iDNW%2F21BteabpuczMQTbgkpvBmQqI1c7RQJ27Jr8H%2B0u2wpq%2FcPTG0qZdlbGH3Ztr7QwzGcLHevxJz2gbwEGg56ASDiLvtT%2F47H78sJzN4mEJjlEaPeSeqV5HfOu%2BIBfwyZKM6hDaAm5lmksOzJAX5q90XSZph28TCGhfHIBjqkAWaH29H9pweqjH%2Fih0kynaUvcW%2Fl1vwEDuQqa5GI8IPK4eOPIlvkv1o6mkQbC8WO%2Fzv%2BtnVj3tGR7O0UHV4NS6oh31%2B%2BLYauEqjuiQ4Jzvx5QqH7XiRNyXOysBpuI98U5ZDKvuVEA1TV%2FqHeiLAiNewXiChpodCYGEO9lt2Ixif%2FXig%2Fu9URlBvv5jTL5%2FX9VuTmUgOOX1ORLXecQGMKPKtAUble&X-Amz-Signature=07d601a9f7e48978214d9111c5928989e626732732f1006fa485fe0dc5dcfd9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

