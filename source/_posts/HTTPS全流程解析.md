---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663R7PB55%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIDks87EZcuweVxV7xvtsd%2F8UII%2Brj%2BPecLZcS%2FYel2q1AiEAjtCLBqPWDKnQ5VDMG44t594VoiMVWOh401w0RertUQQq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDBizeTBRO%2BLXlkX05yrcA7T7izi%2FRL0lg5EEC0PYvzZMCcPzTVbdNRO3JuFmRbCeXuiK1X0M5m46upeLMT9mc9cgXkRF78sRWGdAI%2FpB1hQ4yma%2B9N%2FLElTE6XqdUkeBznAUJyrx9HYCYyuBIZrjXl%2Bx51LwDuni5Pi9Jj5%2Fz42ACI9whnjaTlsfZz0Q3XZ%2FUQTEqOApRaae9fhz%2FDUm5Ji5fGTYSvFMEHTQHUhZ5gxcfwkoC7Qsd2KnztvKWXzB4gZc3zwVZDPz67ROnUhXEkSHGV%2Bok42wdC2OsLBkArCygSf6eKKWOiLV09IvGK0aGFu6dU%2Bstt85HfRbpND%2ByY9MTml3%2F4%2B%2BaXXEBziuvvDO76CeqFaYBEqj%2BlBBsqUbyvkS%2BmoweBIsDO3naLx%2FofsJR7geWuCdNIODfUtAwX1vSbyQlhHSFQEjBYjb5RHRDEkXXLnsD5SeMRGhLdwj2MqCJgrzUSzmRz%2B1hgvAzt3WvnK9V0ILr7GUPOiM41S%2F0XJvQ9QyfiMPp80L2S3562qhhiX%2BX9p3eJ5AM692eLBqneWt4GBRMJF%2Bej9kcBCVVa9ogo7l2YHUS2uLk7Np5hEILj0PjQkLv%2FLy0tbRFn6NhAbAWkXxlRhRicYSe2Z%2B3ZglFtsgylVFkpQ3MN2mq8cGOqUBwyZIXH%2FRDQCVG%2B4DsfF3E%2BWvWtwzBmEX8gclL9slLzAro0cdmzBSmnJkiqNqe9u4QRCT8YJuFVB65%2FZd0F8Id0tddiw3M4CacI89woYpiVrgr9G0CrP6e6F7kl2Rkf%2Bs2WNyIPaaP4ej1Wp5aIeZ5Ak3S8i6iqBhpEtMWzSKSg1jtToXdY8fYA7PO7LWJ%2FC5rprH0M1fzLxJUIYMpNNdsdaTmFUG&X-Amz-Signature=b17e646a0daa761e9a6c582c48d01106899e1355acd102902f0e0e772c89e0e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

