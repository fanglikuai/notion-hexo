---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPMNT4CG%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDTv3GEQ3VUPwE0S7eKQKqKJJpUbJvqox9nTd1McU7bAgIgWTT0yPqprtHRSN4xX25do2V6s5lwE%2BJog55hgznXM4sqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK%2BD%2BjFLtDsmn%2F8PircA3%2B0ObGOcma8IM3r3F5XreBC4Bi65QOuRyghgvPmmKivHbt9qh8XGIXZ9p%2BZzfME62rjpvlKYdZw9CO7mEDlszkVXJeRBtSpMc6h4aVavhKGoGIt%2B0KAIGZzk3YERe6l2c1dtmmhO5aLFWapjbBY0v%2BygrWiJmuvH2yFEFpMVs3YDNjWgg0iWvkLYC%2F5jJy26TtALj%2FF84beTt4VZ%2FkI8YRdr82ScOYMJeQvA4j1Qlbplu9mHduFfiHXF%2B08s3Thtq6EysV%2Fo993OleXrf4%2B%2B6Ui3Y0ZcAmabNh5Pbd6Cx46rZ40m7mRlDcA0tUCcpTi6HDIl4At%2B2wFKSXsWN5sXYeaT1Ih9orzmxdCoNg49tuimfWLhQ9pg2SK72xkzRTjYa0cm2pJpPRAu%2BUfj9rrB7ksWgFjpEyWs73Myyh5aaebLVPjYBb3or1OQYURuvguKeCocLINc6o59L7V1bg8f4MLQoiUYYOzDTfnQvXv%2FSo%2BSsrEKuROx3%2BzJvC%2BPjT5pujXcr135gNulBO6yHdDPf8zZ1X0106jZO%2FpIYnxW%2FA1mlP9y76gvqu9W9VMupaezQqVIjtXYFQvihvMGeA2eyNQImFTz9Lkxow4y2s7BTEj8Yvf2auTgm2nkiAIMNDX5cYGOqUB9UHUUUTJx0HWJ5t8%2Bv0tJ%2FlWeViz6Nnlg%2BfIoHOe0hkzLI6RZQh4OkUPVn97PcgOYHKYNUVWKm%2FUQnNbx5ov5JW4mJcya5JwcTvcbJh1tjJ59RI%2BWISJ0qTNQcG2zX0CrTGB%2FaFg31Yc30X04P4ZR0D2V%2BjxhzU1z%2Fq55W4t6UKqMZR%2BC9DSPDek0ZIrNLnr0N8cx13AkWg2ir8q%2FvNJ3JO9fgkC&X-Amz-Signature=d08b19df572d1e8310c004177c5cdbec662724455ba9dc58c6da479e83a59d05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

