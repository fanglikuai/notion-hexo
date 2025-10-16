---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRZJBB42%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCz0%2FDdC29sNZ2dp4zn%2Fz%2FUKtQU91xNvE1IAB32xwOqgQIgHs7XjnGqqzTvgurUd2K7RlWuVWBRPKhbKTGjpOitXskqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHZWCPQevMfmX8CEVyrcA2wSHbCVcy5BhK8QVY0Wj921W6zM3Bk041KRRkL4kU8aMtXLtXzpjRHu1VufLRRjT3nQzi2G1PuBQO3IPngOW9yRDpVLS1hvouBgiMNxRo9L43%2BeObnll8S%2F4DVxcYjB%2BXyMRNG0ey6u9WMe2oKAqrlBKmgIPyKuVBxfQLHu04JmaP9Hfr%2FEhs2QZnGRlsp3D9QDraGTQixMlYlK7nn1uqcR8N6uWHXGs2i4krvt1d3SWlQSIjWW1Z7Oixw6w%2BEaqsgglGyEXsE9zgj1PkzEchoSakTxRZn%2FUBqwCpqMeOUf%2F0QmPqQUn8YvdBIya%2Bb3K0vMGaB%2BQJ3jDRL5i%2BZJIJrcFUgCO7KKoCeCNIqvfpxv%2BtQ5SWJdOUAfoHO1Bz%2BJC8AuXR40fh1xbOQ49yXzMV9%2BeB19toiELF7DmF8NVvl7lBJDyYGF8BOdpEjnXMKSWAve68B4Vani9eAG9QAA9JU%2Bmfs73WNPbqPReME74PH1WS%2F8D5yOWAdPj2x29wOO4yOzv5wr42mk0IP1gKR0amCCHB5OLP4lct2bIoI57urLqT%2B%2F9t6IMO6fSeGuKtLQX2EnWdLdriS4dCC99ckZVBgIxl71POt26fv%2BQof5ULkjMApYmFcWQBZ0rSW3MILYwMcGOqUBN3fmJYqgUDFUljrGs3LbQokZOaFEM3EwHxLKvCTrrDKGt7ClwWZyasgHPyFubwUaLZ%2FZjtdrnfZ8cVgHWpk7cFZUD8Ipq%2FJ%2BRNKzHYyrrLH8EeTM02zUfET%2B%2BXYrixbUKNXLObCqWLFR0SRvgysvFfu3Kxkf0TlGTp4F%2Bq4p4nO4afckCVMTIsksQuijEunRp8g1MBApVGoA0ArEBfwnNj%2FYzxbc&X-Amz-Signature=3d21924e402ffb58c1b1874733ed042780de399ebd786c481aca55ed15309dbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

