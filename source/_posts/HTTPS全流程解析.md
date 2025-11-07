---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667G3LOTV4%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdSNp6mMGKs%2FgKue7xUi02cizSD7Ftg%2FszGEzQXhAM7AIgTRI4Ueu0FiPm5jUzf8d9fJq5G0lK%2BqhwKyYUrAuhCc4qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIgr1R1%2FFYQtlc3x9CrcAx3xeObIzE9E4e906bkfJD2fAnw70Wq7bqel9owwXsFP5%2FUGZ9eEkfwWQ3cKdFHOsRjiNL4dOvOx8Jg5Ci5Pn5DiHmnTuroC%2FrGnqguz1USt565ctbRBxIZ4nYI%2FMVAsXS3lpHh2v9EZKPT0aer%2BPlvYznSSubFxvNF5L7aZHH%2BQgxDlOv8vsPAT946MTtgfk0rjxW0GGIxOVj%2BlfNk2USbT6rctZ8OTuLLyuFA6h0CfnTSefSsSNX8lJ%2BFngqPTKI17VNFeQP4t6H9igKMw%2F%2B2RgonVrErElz9IDQSmD%2F2BhFdI8WTNeLtSs9LQ4zeRQunZLUqMK%2BpxAo%2FlAjRNz%2Bd9IctyyQYS6Tkl9kMS1FOq5ICZ%2FBXLjLZrEgldKRPqBbcExNDno%2B6KDCvmYyBBdPxBUyhOqbz6XqQ0U2dQGt88BmCDjRy4as6ZpSNqZrBFyz9spTDCyVyRhYjgqUgKrTO8R%2B08Ky9NO1JxX09zIosTz2a7JxJ%2FfxP7HdxgCQmDfU1cUkkhdPIziXKgSsPTiMc%2BKJ%2F5YvTc3BBSNlQywuGVfgRU6H9Qkn0qQHHma3j4yepyGGp%2BLpQ89lpP9i%2FDRXVtonLrGqnsEZNzY5Mk96jXcL4yJpu1OZf%2FH9MeMKvguMgGOqUBqSA9G3qwcj3yUpH%2B5eNXQRiEhmkvj3dntBUaP7cFAeYwLwnG2gPWJn2DknqkpsHP7vidRvlPpt3CcYYGnip1uuM3GCpNmjfHTmfksRPzQ0HgbHA1Xd5eZqZdhEI%2BrcfWxQHX3y%2BR33L1XK%2FU5T3ocvGmJLbrFiPt4oiIhiV4qjj5B2VMDqS6R8Cc3cYpMMtKraCswZiTVtOtSF72qQoivJyhh3kO&X-Amz-Signature=47b88db8225416e4fc4c7d914c49e1bddbd3b3097d1d3bcc4c703c6fb34f3c81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

