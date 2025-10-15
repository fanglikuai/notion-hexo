---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VY25SN43%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBVHtzif%2BhUmdWjqTCav%2B3538vHp%2FY5GXQ2ah3QheubFAiBczjIasbc3%2Bn798%2F8VDOJrwcOohvIJC4kLQCRLBM95Kir%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIM52s2MY4W7QJqgyQxKtwDmDxB2cNd5fTH7jtQpZJTZtHzr2aQgiArdisJ2%2BERJpNhJDJZa5205PCCAawzqUnvBNT4NRj3VDdstuOo199ANtBiuMSOYWde5W%2BWkw5g%2FOv%2BKqGxAF%2B7ku7IAbzzcLYrEut6TUaIaLoyBzIP4D5d8j8laAD2Kgp23qWNjVxNGj2ufmfr5Xy6pasNx6lWgiq2Hnh6tZ%2BJktmYsfQDkvhS7wzYYYu%2B2r7qyDOYjzvSpraM1ESDL8tN%2BG61iA8j8a1gBU3iF1R%2FOnaqyBVprrt9XOJs74JA53HnYCm3WynmAX8YhjQL2vefsNXpG2xlSfVHy7Fp%2BR%2BfTbvfgm4MMQQfjYho4ZMoJK%2FvtT%2Buh8en1rTQxztXL8wh86lBDUN0t7j0ERpIInmANPsUMvf4fRfxH%2BvrIAD3TWCoNMv5AuOfcMkW5aUHZ8d%2FiM8e5v1bvJl63XpjHXe6DIG9gw%2BmKYEiZj5Z8HzYpTaUYiz3DxzklzU%2B0q7HPglnn6KKJ6WlqsY3xSAiTldF9sui2u6LnBv2w2hx42OK6N%2FX0gQd5MxJCkM6%2F7ZfsvQZJvBK6d9ifmnONA621f%2BEOh641uAHx81CLpiijD7V6yzhC%2F8N23YNvxKLTdsgwL8it9EZ91Iw5Im8xwY6pgFtaO2LNl0aBtle7XKBirG%2Fo6N5hM9P40zjiQalVCzncZCalYRLBEGOFKdEBaoGgp3PnqUEnOsxBtBcOYtH1ls2ky7Qnn%2BB%2F94KVxq6HcdS8fpknf%2BXcpf9huJjr3GohUmnZzC73VpUDVdg2Q5IxPXeEOWR%2BycGxsX6993oN02yFBqW0mUfLCotF6S3N5KZyRQ9Eef0fCSJ9fTL888mxWnSbmi4b8Xk&X-Amz-Signature=b86294644d29c7cf364e5902f30e38fc67742b1ec7b445acbd2b79a79257c0c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

