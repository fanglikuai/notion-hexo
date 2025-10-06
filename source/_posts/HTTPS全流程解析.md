---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FHLRK6C%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGIvr%2BKFnoiPqynVrl7JJI%2F6WbqlJSFDGs62GBfYhhf9AiEA5RGRl6SnUHaL3Vo%2FvGddwEjwNDdaamueEfGj35JLIhYqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKZ694HB6%2Fbje2QvoircA1GuLrxb5xqWxpvbsn5mJbnXH7E1L9KwjxTOlXGZQzk%2Bgd5sKJB1gY8mTG8McwMmyPxhPeSiZ%2Fb346%2FKCwBsph2BKHUvE68pN%2FO%2FAIT%2BffSpM96k0jDr6eD%2BKeo%2B1JgpAgPMIXFuzrBI6NK%2BZ5DMHDTfWZrkRqv5moM9iwQr2EsKbJvbpjtcYaBbjpYh7YqRg%2FhLbhSZ6I7kHQMqLVA05IJW0qRgo6YlPsaUt55ukZYHTtUS%2BoEpi%2FIYd9eSg%2BBc%2B8sysNUUrygp0JoOV3W3ZES4xcdfNKWn8t60ko%2FmeVJAfPbWzG8B7rSBhF3BQsR6BLZlLV7nnakwYGO5SGDg3SHpXDem3hwXgaX9VBWEVKzxTgeUnveZX9RyybRK3x8OqJmjyINt%2FxTkCRo8ml3SOg6Mg0ORNIvT%2Fz99nH8eMf51vviwm5RbpxuDG8m3IFVUtkPCpomTOhVk3lBBJk7hANoTMBHuaqD83u6UeDf5baFo5LUTD9eHoV9p5K3XgJ1NzDNpnHL95rn4Z8e8Db5%2FkHL%2FQgrE97D96SNJfvVyJ5FX1Zz6GWcpMeMnmgvVAdnsY8GQXY%2BePi9MzpVEmCpMQ%2Fm1nf9JvdayTZBNm04Y782MQjZVuNcBGfpSPNjwMID%2Fi8cGOqUB9oPKVTiRWzIcvCU4o89CT0auyZPI2M03la4%2FfTbydCqFEuEJDLrHkB9pwGnRvnqNOSNJ32iLiekM2rLH6J0e2N41YAzf6%2F5zkj1WYWXXUqhvFdX4joMqEK716hsMBQ%2FZzmX6SGVVH%2BThwtWrbk70NhyOGZqmWPf2yEyun1ho2tvhSkj%2F7z%2F9cWBi1TaoQ8wvscU2zZpo0BX99FFdF%2FwL%2B8HlrynL&X-Amz-Signature=6bb7fa5251ce5a7c048896b653723848bf25cd34890a204d8ec6d34c9290dd54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

