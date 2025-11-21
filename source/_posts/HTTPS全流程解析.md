---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7F6J5M7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCsi6MSmeeFLde0xIA6JvtPBG7PNI6K8r3f14kzsutEHwIgRejugzFGrfvTBWkb5tBwJ%2BDky%2FyBf1mwNIddIUutCMwq%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDMkPi7zLzf4Mx0b4KCrcAxPry%2FAf27DtWHk6KuuErOyVOHPDqnRU7dAWT1mjgIg11NehaBkp0bqb6jwfYmZUvwDwEb8gvME4Kz1BTXiqK0ISQbys%2FjsYiNyrY4qExZ5Wri4kY%2F5W6hnyvpSCesy9msYhPSv6100xM17HJI3rAvVXr8SkaH%2BzrFLUR%2F76TdDII8M5zzMHk1hMkEEiP1WsCN%2BESftagTQHzg0LLmbjsk%2Fs2WhaGZiOTgXrgdSEPgUJWmSptcOkbJ77X6JF140WgyrszQ6Y%2BCZiPSv8xr36ojvLAAycQxZWoGuHY6jzUZy%2BLTAXZHNCsWKdJOtgwpx5dEFIXXT6Rxdwt68BfWrhw%2BmEIeOIpEa0kbRFAbJx%2Fyl%2F0uY2aM2Bw2I04tf4fOEFTPZOt0CkALofL%2B6OFnw0O%2ByjFIOvFAhChI0Ra%2Ft32G%2FhtuwUCmlWoKN32vDEoPb600uo16gRUnbs3wIGi3gQYSgBCRwEaWaNe2eF2dz19l27hfjwa%2BmAW04dFkwbNM9s01NizQH%2Ff7q9ulBA9JKTNi18z7DYzXpWlWctLz7mzmawbqe4AO5XIOIuEs5cz3VE5aja4UgMq1wRmb8E9XDvhbsibkCrIt1JzYFxwUY1zFZvb9LIh4Jbzd8mE3iwMJj3gMkGOqUBPwAYPxtXsgyMGuQtWNnr4KxMxfpXmxCCry4yw80Xhrq5GOxohjTStT6YJS%2B8ePLvJw5vI7BBD0WrfuOjK0qf6%2FQSuhK8U7iV%2FWetmdLNJPT2WmYiO0ZR11lEw%2FvhXheI1M14nmKirNJ0so7MH5kgx3qCUc%2FMy6ZgtKmE9sMCu9bnbyjPS1OxEJwP1EyBJqZ0sub3d5Sndw1oH1z3ef8EnVrkZMtp&X-Amz-Signature=f4f234e2315e529c94f25ae17c3a0a770041a69a786168ec84abaee8ba41eb96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

