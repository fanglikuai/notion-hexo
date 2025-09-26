---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXNIA4TK%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCZQCVHVLOxubSEWSdr62e7%2BvydJWfE5oX44n5qZI4DlgIhAKrwBF5zTQvrgT%2F15hfwiRL4ysNjOklCknyZgdhHHLDmKogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgycS%2B%2BXwVxvWzXwBK0q3AO%2Brwbs5RcSfMtEif0Bi8AfiGnaBs%2Fzn0l1ZFCubMU3XdBqx1ablKik0o915IcXPdFW4o%2Bqk359KDdi5PlJ7hHVuflX9feMD2eogMNMUY0%2F8pAXgIl9ByK6cJZYbQvSjbf8xG%2F4E5K4bjA4ZwY8vT4zmu7s5VKUgJal1gXDUBfl63J9efnbXmoAlwTk85YDQiP5Fz9%2B2I4vSjWuanerOP1HleaKNcmYbLyPKkRKpCTP3E2%2FkqFa6hUVo6BGGMhutOCnhCoPaJwAduVhEE%2BzYAWcd6i6szmp4EiV61J4Ufpfsn2kAX3UtZBQtDYd5Gf5FX8ponbcU5jAzlgDSdWfwf8YVEOr1U3EKRrbJq0K3WDNHp%2BpAsyHsALwnmo7ae0nu%2FB4KHx4yGL%2FbO%2B%2ByhDK9LMHMa1Pzvmne88dJsJNeYwWPjLTOiSkp94vlI8fPwp%2BpIKxmfh%2BpMhSaJUuCTH53lGSKsVH9vIBCo958G0VM%2FCRXny1Bp%2Fm6SdQTpBEHiSImIUa8Eck6N2w3MbQ%2FTm7wNRy1qte9OUd4auRvh3pfnwxuLk%2FJ1cakEwqbYEWXevRlgNNN0IXo1HYvuV0GPVU6Q5JAw4zv5KJVtIwAjtzoXam5kqBMTWiZNGYvqtm8DCI3tnGBjqkAbaZxmUKn1E2sS%2F%2Fa6yxWqqRui2y9Vv4XLy%2BMuDNXb3X%2BvqR02izg5uDbmX1nWvcfPLJ08iYRuIXhKIfhOTPM1qQkj0%2FLtQ810OkMg%2B7qOaERwtvITUGpqEQSHwWLvsYwMvkmYui7xNTWYOYgYKURzB4y979qy8KLhpyDIbGBod9h9YlKwoRw5VkRYFH194bWiIZktWP7Jp8Lc%2B72gk2vcSXNK0H&X-Amz-Signature=cb1931fa7caf5cdbdfdcbb4f20ef56695af9f85dde8a60e45e7cc6f8cf297fe7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

