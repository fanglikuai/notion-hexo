---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQ3B4NV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T180053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDP52q9PNTe8zWZ6dpYR45PtV8vTX%2BxEDhGsbHizYgpjAIgGqvlYnkH18hPXaOpjkt%2FCvjJ0kaOG4%2FUi%2BtIPJPSnHEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLutctyKGLIP0xFxFCrcA%2B2SWAIqoZE43qq0hh9bKKMSTf%2FgWElERQ8PBsvGiWm389ztF%2BxcOIJM6xFw%2B5Uhgax9wOqtvgxTu6kerq59uzNNwN0NtGBguojEABx3KeHbocAkI5EkZshfPIXrhG%2BQKK5URFJNlbuU9PytANlHbysUDwztaaTKeVqccJ7WJ8WxQ0IGPtxjlAK9bZPG%2Fa7xMk8VFMdMHxwrOORQtIVCPwI0keXrJtIo6D5X6VsHI5ts23tY%2FySQP7SwVywZC85Vqql%2BCfC1jU7JpoTaROHFfRicEsg%2BiEeSBORo4blCUPTl5XQRHftTSly7fmQfVDN8Q61zWzh0lLNM5RH6O6aziaM%2BaVoZTCbe4GPTAJdWIlQ51A6If0gAD4zMnZarpkQddrAT5%2B80%2BvTDdnrKYNHmzbgnWKvz1Qo2KDyDuST2VlRLbVVKJXt9eP%2FdzZejSOxMPvuW2mHj7kOqZKUbXmAE%2F%2BIorH8c3L6C428lohG4edQnWk%2FnCDu40aGYAfzfFjvXgV2mJEdIJmJFUvzFEk%2FsWYDT%2F8%2B48gBFu2LpRgEkixOxYR4D6LvVXMstgMgbK4rgS5lgIUXRBI2EbcJE2hSxAYVz7vbtATsQa4%2F2IDlOCtP3a3JCR5dtZcNqozs1MIvJgskGOqUBSj%2BqVURJyVXjLhV2RFOf%2F131gcvqQnkQCSr1UVwjW%2BViftDiJSpD7uy%2ByN9BAOMgQ4JcErKe1ZBfKtu8yOzEqli6Ni6kRoQkPxBikgyfVwb%2B4hbK9dbfIi2qbrJJRjUmziVdTq6%2FOOJ%2FctdOlwGAV6uPjgcosXWxaQsMxNVnq%2BUMvQ8fDWw%2BqgDNdHVniu0dy2MxIIY4aG7k42hHswtDNSozdYkh&X-Amz-Signature=17c94ab974c49f804cc86a7d43d98b1d4b22dc6fff201fdffcaa8071699fa6a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

