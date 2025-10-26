---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4HY4B3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICWuks8%2B63%2Bs4ZMMd93qGMdQAVBlA5Az531mGeg3wUi7AiEAk96vETE7%2BZAsqouef148Ux6d3aMw4pyI%2Ff0Zxgt295UqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHR%2BX1X%2BMEDdfI4pICrcA4Au3r1Si%2FSUhETG8Lq8NiNTLp612jXuGsL47Ec%2F21PMb%2F%2BQZ5chE0JzVK0j2n0%2FUetMbCBBPXpzwj77OZ5P2q7m2VDjxNWcO7Vj9jnmAxOy3U7iAJtidqcbhjpQXJx%2BW81001Nl1QD2Sky76lWkFBikLkqrAc7KJ6E44JGnuP8TgqyTVD6GZXDPn0mQiU6PPoFuAJvZK21fslp0JsrUkQFH7t5RZZyabA9vZUiu3uLYD1YTLKIQ2xKlDMoIobbSFYnd2K1G%2BBF7yH%2FwHYLrM8SpzmrmQs6oWDjFCyLS640SpVC9Tu12MZIwwRFha7jTs4M1LJ%2BdBbDMqU%2FdnHepdnnW%2BA4qWxT1i8PHVXeqv39Fv7OZEqHUyrmmSaYkICmuoXCVeUGKJ59wko7Lh5u8QDSugKESFqMxtNq74iLktThs7g5mhfoFJCzA149TWH8r2iX6TrCe0iMzqzWebSnTjDL56eSBtlUimctw3jGoeCwA16MOn%2FqxaUgdyv8YPU2X66Ffgf01rJ3vYRYQabkcM0danHF4XGiEKSBteSByxgI3PWa6DCARtqcXMH5TgltxMEqL7r%2FU0E8Dn%2FkOive3ifBiQKpz3VopgHdKFn3ntqc7lRsdN32Mju94n2%2FwMJrO%2BccGOqUB2jFOWZOIStCaOhOs8BsQR%2FYVjW1chLtjLu8ttHmqiGi7HX83eWq9GuhrWCFX2bxlxc315KnP0U0FXzlOiCKzKyC%2F3ukyVNqVcOL6qibR90zsVRwtadqkep06OxyPm459uapDLos8vGFwcitGns9UZmBUuFvAjV7R9xxYqc81VU2zBa9onrAzxIsi5US7XlATZtk5jEcYO1AJqLX8StFjoop%2B%2BPe%2F&X-Amz-Signature=992d385c1ecb830c45efa79188da7f66f785a150b427f864ae679630d26eb910&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

