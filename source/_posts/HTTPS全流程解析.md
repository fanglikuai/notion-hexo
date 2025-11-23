---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RK5O3DC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIQCuzTtM8KAA9xsNK38bN7lWWuLP8S%2FqGTgjFA5Wm8xD2wIgGomdxEGcBujJTzv%2FvtUJZ2Vp042PVMBQFZdK1OLSjmAq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAbXDGi2WuJ6PqW9oCrcA5b%2B1sjec5c6AC9pIA%2BK06ZsCzP1vEjm0oAoHi3nEo5BmEdetUCXr7rjJRqRg0BknK4z0KjAtxWxgSoZ8rBKJP6t9aRPcX2%2BCaMM%2BERxl1YyXKyq0WEGOLMtrbndiKnxIgpRrhbjTmF3yd%2BEzOEx4i0dwAATcxyaLISrVtLh%2FUh8DoXgEvRcKatRF3cqcNljoyD0Iqss6AH3QUzVy57nTETlnw0tQzDPEP78%2FXImKVNSVpf3ETJtfiDZit%2FXfFQUoYseuhFLZu1MkUcnwhLyAeAFA4t%2B6JxwBOR93CptjqtCU2aIz8tvi%2FmqNlJiNq%2F1rsomq%2F3QxbnIxaBeZ0gG0oAg6dBcryVraGb1UNxDBXbrNqeDouqNMgmDnavJJb8D5dhiExGvN%2BuaaK%2FPglOpPbiFnToKHUmIsnvR%2FM0DeaGcA9u7Wt%2F%2BY0DCSXgxj%2BRurDfTci4zyO%2BPqLDF97pX4ZYXiOUGYtveHnGXNNrWvYcCVRl%2F88%2FbLNb1DZmtse%2F4IwmhIqN1J4Q6A2NbmuIbQ3%2FN80BCUzlAlsxJ6dUx38v2cE9Xrcz9KNDS7HyFVRUzwEbIlJQ9PipHvTb%2B6ao4h4NojmLcXAxkQx9MAxLWdPAW1O0h7S1v1aXLuTOsMJi6jMkGOqUBcUNJehaFKbZAZ6Gmn7bfP8NzCx%2FsZ8IAGTzmzTGrwNbjzVCMvU6qp41co4Ay8yVSck3E0DAkHYKlshkFvH5cbGhiF%2BokZN5HT%2FVTwo%2F%2FUjw1OJV6JkoK7x1NQdriWyhoU3QOxSUM0U3rdZbI12DA2S5B7wlljtOB5tzqkmfpai4LXW3MF8jh0viMDQwlB7IcRcG4mHEDEOSpEXVwjj2VFXTUoOB3&X-Amz-Signature=7e168f391849c79f17e342cbd4ad7ac0196f152664d8e5bd209a065d3c5bcfda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

