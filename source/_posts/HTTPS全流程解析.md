---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ENWCXTK%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIDQQ3MrjUpvF0I0YW5zI6kwT7jGUe6AG7t1v%2FVMDUyNjAiBBkvGBBiUXmNbrLQCvZwmmU6HB%2Fkeo8%2BxJzLnDDdl5fir%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIMkXOaTkB7yrJw0ZPJKtwDKHD1Z3eCuzSkPSCSY%2F7Oa%2FnLQuSV03vv%2Fzyf%2BE947FEo72eL%2F%2BKTsL5Uhe1eSsDU1W8SPMoXm6NjzqhIkeOwV2OcHuQWhuaVLf8bEIqd%2FRaeTjmA%2FksZu%2B68tOCYMel5x4FshxtWR%2F%2FzXf%2FMeUXktKOGu6YF3SlU7rS%2BAoMzo8kV2glmKLRG2yn8fCOHtQtdGD1mf839xpJXR%2FDC5dz4UNw%2Bwk1tXKNaVHsjXcTr98aydT1XjQkV29B710MUseHdQSC3ps25MvphGr%2FXEye4gdTEqg3Gz76%2Bv5w493CY3LXVbN%2F%2BBF3O4TC%2FVi%2FxW3kYQAev2ONrWpxS8iUdA9mrwHICCzj3xsHMOIzYueoI0KuyM0sfXRiK%2FXmTzzUqydMfJcotHNIZzwO5T7H0Yz8D4tW1OLrABVw73N%2Bx39LPQo6ntS9zKsJS%2Bdxoj7Qod4dAvT19jydTWyTnhyZtSgEHm%2BaRdGSB4KRis9O0h9h48jrPOh%2F2mqcvUaA7T4PWYQTRQuk7AH%2BNFBX%2BqnOaToUvCzJ36kL9vqZ82NBwDy7bLIHPgA49YdGHf24PNjXBBDTFSAR7Qn1f7vRQpdr34JQcqa8foNd%2BXhF4FQ0OeJU7w9cis4eK6zmZd2eydv4wr4TLyAY6pgHViQhpA%2Beoq0FYgCoyI04SkHQVRmHQ%2BE4qrM5dsFgLLx3gG1MKS6w4GVyhSeVT5z6HApGC%2F441YZWv2%2BkrY%2B%2Bdq4VA392iHgYeCYk67qBf2DHP1z62BAk5kbAfb9%2BXlK839SGT1Tb0M%2FJeAj%2BKBNVIP7bNgV9NwZV8gj%2BdHEZ%2BFADjLlIdK%2Fet9%2BXxQ%2FU4H2nyDSUhDGq7EyJXX9NgeOcqmGpHoRX5&X-Amz-Signature=32de46cd0d48b2adae0f39992c7a908c0e58d48cf402a4b6a871996609db41c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

