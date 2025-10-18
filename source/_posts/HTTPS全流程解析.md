---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677TP7FRA%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCeTAJJVaWv9h82G4yajd8YvJqDLvHwI0XFgt7b5xMViQIgLE03DC1kIxOzZUtMdXx8TPZXEf4ucEUpR%2F4qb9%2BHivcqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAasM4N1vbB1oBAObSrcA4hrL56pcxh1QrtESWycjDAE478yEwD8MYWUIFCyavjvB3825gH5Z4H%2FKRiMiKXvyRU3nhb4BmtbGR77ujzWV5mrS5iVkQYgGKdNGLz3jrCSU80WGSm0d2yGjvX4Bf5rFb%2F%2FFl6F3MumuuPBB7kY9sGla9YyGd%2BJshvvEKGJakfiThChehyzAwfDL5STNoFHtHq0odqYF1HWPLp%2B4n%2BZM5mbLQWxJPls2Wn4%2FWPEE5S9g4S7Xo28hxG0a5dPe7UYfAM%2F3dPJ%2FCt9Vl%2B7WByWMs9fHJiVS7zS8NK0WDfONVNJLZlntTHCFyiHGOlT8gFGZZPeodaHzC1LAv45txolKh4j1NtugwnDPYP3E2OqMO%2F20t8pM8ED5gtTveJMJExHquhL8TPLVzXRcUoFmEifnEouBEHoJK6e40Y7Qu7kGboqxr%2BVJ8YewHcRRtMD0CHIjQOtED85mw7ordckHyv%2FjNTZWucZQ%2FhqFhCNCMHMpe%2FSyPr9YwsHAZQWZDGv6HIDLd4dyjBa3TuAYmdb1egGWwWTwKsKOuFVfhniXrDw9X2FW48hN9f6lUP4v6hWLOcQPUFKDnFSEkI5rd%2FbvN7sCXVsUFcYFc%2FAwSyfTt4E5ByM%2FF6Y5miazc74YaE%2FMOjkzMcGOqUBCpX2E9113GTMlaATD5QflAXLU6a8Guy5d6mH4B8RjiKvl9kizc%2BWU2bVBxvUGZkEIKlGVowoMdRtToI7UERgOr3K4lRmEk%2FCwyXdtjBrfbF1oIhI0NNZXK7Z9LhmSztd%2BqIdNLzXLjIN0r2ZCnx0ic7Wi4EtMsx%2Bj0Ii7QRsgz%2Byl3qirpm9Hhht6qY4Sos487fAeu66hLyuTE%2FvkqYiVpcXV8Rq&X-Amz-Signature=47971d9848149dccb7d1d4bc0cbd70ffff32f5e60dc924ab139ea9c12de76f45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

