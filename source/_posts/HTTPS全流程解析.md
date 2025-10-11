---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6BM5BMF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJGMEQCIEyJT36lv01J8j%2BSv%2BCJOvlXDjlLVpfjHN12GSMklF1KAiAGyPiDCFyOFuJ6uOrW8pZ8%2BSqTcl5Rsd9G9SXbXOEHOSr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIMFBcCh4FD%2BJJe4NbRKtwD8DPyc5nkqdhh3R4AdcY18iCkCA5cKT%2FsycscaECS7rGGr5f3r1W6RK52XneuGD1v%2BQAgQGPEpkuCbYZNCioL9f2%2FthaOTIt%2Fb24%2FrXv7hTX0tVysqC2t%2FjJRHvfg3o9FRhq%2FQ%2FYidoG1iceP2zYCsZ%2BxebWE1Sx6W%2Fz7Tb%2Bnep6yPJDIvPzUpTroTFD9VMDDFoDshCNThSqMojnq8gZLYZPSh4sgbWhPBi%2B9BIrGrtwq5kxFJqyu7k92R1HaN%2FD6me%2FibLPnHs1fW7BZ1AuVZo58bAqFJom27s0negst8F%2F3npKabgxQfRTvSS%2F34TpnmjxrfYSU9XMy4LDubV3AV9l51zTEg7Q%2FuN5BR0r0oLc82FXmJDICjNHA%2BR74TR7HALfvZL6EHhD0g6%2Bt12zQjFMi%2FJbPs0stc1hw%2B0QmZnc439izw2rcYjmibB29j5okE2WhDExiqaq749Y57YzUX2%2FTDdl2leMMDrCXQi6%2BOV%2BiDABVuHdl3p3n10a29oKf%2Br6qJ2gzJKmYNcMX5uaHr94kC%2FAPOMAYLBONPxS5WXR2cXcLKmlIwzEZw5czlShdkI2FVzFL5Pb61MjtfWhR%2BMjNjIJIZm2OrFEdqKudvVNsCwLkWH6Hg4Qd3l4wh8WqxwY6pgE7cNCp5WCKvxxXQj%2BtM9ek%2B3rlnKQ3njRcB2tlD3sjIFONlVCHrxhvHITFG9bzRSrkzC7xNwz4rRQU7y7a%2F341bnmmeAAZGNaou8h4cQTcR4ukh25on23UrnAuvGgz2hKLWcbxYp2KSmWdv0%2By0LR2ohoqCGXDH7N6pFiUam35nRl4YF4ppOB3jyDXEP4KJL7fB2nuf7tQbZA0pv%2BOSGQrqTuo5qqd&X-Amz-Signature=d68eaedc996389a2bc42f9c432d810cff3298be0d23e1ab89e65b7205b69e5ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

