---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VM4P4LWZ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090819Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmj41ExhqSr1BgnyUIWRfqxDMG7fWB63XMpF%2BNLyV5GAiEAqnmc%2BpB5cJnbsMfMZPyNjRCT%2Bi90lTnJEwiGhOq7ij4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKyL1rvodNrB9niL2CrcA2uXjnbrsKwqyPKdW07JLM9Wpui3jZ4zUGQYohnIfpOAZsOyA%2F5yc8WrlFzhw8x99oOBcYLCNkYY0b%2FJ0Ycxvxfo1pJAVK3XBAHNX5A9vkEinJf%2BzqPgQbUL5c2CzvPOwRL6g2dNh68H%2BESlUnbKxriqpNo6MwBEecN0Yi55wOcH9kNKCbpkOWkCvB%2FWm1ln21oOy%2BWSO%2FAqmexo6%2BNB0f8p019e4V9aZOV7U1wBLhY%2BizFSxwOd7h0jdCCgE9VVNjunRb26QAIvc9J%2Fh1pNmE5%2FiCi0q1RVceT6LNXjSc9DP1omf0nvQcgUhtjJGHlHauEkODqPeFFcRURfI1LSn5FRUP1XK1CFZmidyUYo9wT6EA0F7vYZTKLvArIdSqsCvFTD5EPQ5g6WKWfynzA3cQ6jJKswYG2AkMGSEK1vXmzXvlKROV%2BAqJNmTSRJXtGJfhrS5V78ywvJisEJq8f23tPa63%2FuHkvPQwYj14FBwoSVMk5IGNcB4V4C8haWYBSdDm6GgsB15%2F4dd9CPnx64b7IKpJCctDQgYGmyiw0nhjDvsu1JY6z73M3Rm7QtmmT%2Fapar4TX%2BzVASLSOlHfAgG7FliXHc8hZR%2FLLLs6wH%2FkodHhqyN%2FSbqtkqtCVkMJGTn8YGOqUBXUm20HEiXdSKLviL6K2Pv4us3zuPuTS8hsu0lJMTnwre4gH9u0f8eQFwrw3o3DxiioXgG6uuLZSw0eiQNUSXoaX45n9l0kRHHI%2BWFWkicYxuQSNBiPjQAAWu%2B65Ml6z8v3WMKiOjBTLwKvuO7l3upok9oBaXyRAqDu863Dy3nKtKd1VRT7OnkTCjsndd3c%2BhwwbjqQ5peOS2PpPdyjoV0E8LkZEp&X-Amz-Signature=94f36a36a28dd07819d60f6a0bde9623f31dbdc42584fa552865456294ceeff8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

