---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DBIVIOE%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbmphMPNSBqYUlmC0HbrwLcQG3YfHjICm3WTWRErn5IAiEAlWgY71Ao7flrmiLslSdbVXeb9BobJ1kl5iQSdcM%2F4EUq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDM3Z9%2BSj4v2ajNFVyyrcAyx1CS0qR8h4v4GgDttPwx41PumZ7LmrCAL6LKAHmLyMRCIkhR0D4kTTUR0iXBYKTy5iBrLevWHXxYQ1oUVPpTIfNQ7SU3V%2BXDgNqRBKc4HOTQp8gSGLIL3OwvF%2FIeqiSxi9jlz54nOAE8NlAQTIq7mLaeAc1NZoJKX9ukFvNyft6t3amesF%2BIeCxag8L%2BHdDM2hyNvQd14rLTALxTbe75h1RwtT5R9%2F0Nmj9AWYjmO2gzCGgVcDx2LbOM8Yg4YuMiPoSiVKw%2FR%2BHDBQZGaBugh%2FELchyvANBnhNclrM%2FyX%2BRba3IHgiyIlhYunLCOgJGiKastV%2BknJaSZiqUp%2FBiQYMT0F%2Fltq7wXg92v8k3eVGDwzAkkBVa5yEAs%2FtFmgrQdUpj5Ye7ePtUADmqd8RDskBhR7VoURKOR%2B38aR%2FEywDShkogjTfXMaoSW8yfPkRrJmgd%2FObmQG83Z5AAWXbmfHPOtLQj%2BIwL0pxeAd%2B%2BOuehY6WT9f4ZD1UgEM%2BOGh6HXExaKp9%2BY1zkR%2BGVzCeQuwflBdVFh1ycofHmYar4B8iDBrxxv6VzxeWKVgDqDzItZk2TD1A%2B5%2B61GMDxn0ggD1MSDrbfy%2BUJ7BCEelIk2aBD5UP565IU2MVUUJSMLrf1sgGOqUB9nEg77SbeQg%2BhCX0amcHERsUyzkt5%2Fq40jnrwRjRbTmWEuMx%2Fsbvgv8xnqo3mBMqvNlc0kxa8CozAGs%2FkE8y0VDGbUUgxiJgzoXfuSaJNT0WQ1t%2B%2FFe597HhX9U6bxwU5m8rK%2BuZNMjnt6OJh%2FxzCtALNdlCnehigYWiAtw1W93BPqry9tHh%2FZHyEDk%2BcQAZ7m4KNlKcJXKHxQ7JF6VXFxEnHchG&X-Amz-Signature=b3c2d99f1e3d4b2471dcfdc1a6afb814c0cb2d52a65efe1c99ebae99ccd7d74c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

