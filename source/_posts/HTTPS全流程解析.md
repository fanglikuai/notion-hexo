---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSAJYO3Y%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDndZ4gttarasSnwswoQUVuh48eyykLynN45GYRTgSEsAIhAKWw1sXPFh2ErtHKQvrsDCoU2O1%2FGVRaPcSQDF4R5eN1Kv8DCEsQABoMNjM3NDIzMTgzODA1IgzAN%2FToGuWQSK2%2FZ20q3AN5YHCO%2BQY%2BouMdWmRDnoTBZa6RSVPAZJd5BTdD%2FqB%2B5DT%2FAlr0HcYGD%2Bmn2GevHKSZ09jVHf6Dz1RArXBZHugOZxmEBYv5PIjOxvK1zjKJfYBkqifmJH2k7sDj0IGLD0q9YBSK5tMA9yH67Wbfi4riF6PLn5HFXQqcKN4ARqga2FdFdNv3daSfZaY%2B019V%2FmMpr%2FzoxZofP62H5pP%2BQOi%2Ft4oiR%2B7u2GTyuoKdgXj8UL9%2FfxyupDJbOXxcKa4bREjeN%2BK7SkUn2AL%2FKAYjC9kVk5xAr3R97YmxtU04w1TTeZ9bEoTMP01C7ke%2Fri7r4LHek%2FtXzt01WOXIKRJvFpsd8mL%2Bqr5Nnfdh0rN1RxKUAcNbs2mmbLu4b%2FVFxNDAlyWJEPevdnJXKZCJYc5GVvKB8n4E831k0oaKCyAmJCEB2Lg9f0BZ3VwBakBWEK4t6npWudKF19j%2FVyAjKUp5wBm6UgD2L1mX7GknQuX0kdlhvf5d8OGx6F9kIPXwqHQzS5Mc4%2BWSk9S1RK%2FrpW95nPguxUYj4JBoG5KNblE%2FhYldK78f2nsFqDuUYRtQQbxBCrs7keLuECacqpYnuUDtZ09D7b%2F6PtWyIHPicpA%2BTX26d9Apk7pbo3aroCgCzjD58bTHBjqkATswyvbf6GZS%2FlzPhLRyRwxs7gHvA6fF%2FwSLHGgIGZOG0BXSugOXUxYAOJvGKQ5UN918BgRavNgyKCPatE8HDu5O91JZO3q4yOA%2FVG5bldyQ6zGtCGuSX%2F63Vrew6R2ErxuNUvovgRwc%2BN0edtp8tOp4VObl%2Bo%2BTb1hf9zd6wMzVhihVnX4MiMTk8cBLNIp3AJTtjJQO%2BhwXEkWs7mbt%2FPN33YZG&X-Amz-Signature=1f3716f8308b11c0084326b4a521bae2ce87d3f7d80aecd87a92ca3ed57604b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

