---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7B7ILP2%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T150105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIB68Z9jlxH%2BnCbCoaEtBL2GctrKiDfumKUKh0%2Fqvd8PpAiEAvQi5vsCMM7H%2BotEYXHx8VNn67KfNBrynaeHIQR%2BbiI4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDn0EaKNOvkLOP7O4yrcA1rvA8OPzXm6ch3jWmgsNtAqzOsSfLFDl%2BmVIV4uIjRa1sEHszhi5Yskbho9XxxGpsI%2BCIFl%2FRcKnRC5wn2nX34O6I9OZeVfIdsGtyKbTa7t7gLvJLi5C0TYRUlfBrBJYOa6L6xaidXn3m1O0owBQBrv8FksAceuT2oJCCAmJWLxpzz8upUXdJQ%2B1qH4ZZqZhIFy9Pr%2BOop4T8C9rpihcyAbG2sPvCsNUn4tb0PusmpW3SEU65kBsjTvSkccauw2PsNHUXt4MZkSxyVMulHdeOOuqCPiTBStPKjoJrhVfhe2fcYzDKKuL4z%2BLK%2Bl1qWNDk7u2pTHHtWucW6U%2FXGVVuUdCp9lDKuHnaIUBtAaE72dfs2y1n%2BFPWiCB4cgFU96gcrG%2BJhFxPlf%2B4zPSJpDa23BZ0eyqvuqQ%2FdBfu8ZfFP%2FM2I5cDwtU5boZzSLJpivEvpi7Kysp6KTHFmimPHhpUsXwUya%2Fm6bsMN4BB2pVJI5CZBNyDZCoVyl5%2B5DWaIXp%2FkrFNmowPd4OqGVI5HpeBxImAGdxb8nrnJo2%2FeF046uJLYE8kp6S%2BbDuVXlksUQuySZ7bkBViwWKZSH1YnUPoYR4SPNvUEd8x%2FcWPHddKTKFE90HdnHMkCYlCjxMOe%2FlMcGOqUBbEmfdkD8pa1fJLBWq5ZLnojjGs%2Bd6OknhyiDYyZrJ2eRTVNJ9mIWidgyrES2RvBtoKXnk4MqDKadQsjXh2%2Fj83tKp6DG3jqrsWyEDMyXhQFpKX9sJan6z8tTWWPU0tU7mibcOsg95lsMpnYhQyWM11u05A23VOD71ZAkX0Xp9UITPp2i4e6oTATTrynY%2BvOhOOYbrCTrs8LlxenONg%2FZvxr08L5y&X-Amz-Signature=0eeb3f74f83d89392e9d5e576d5887b17d76bb660101c5d10b244988734dfccb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

