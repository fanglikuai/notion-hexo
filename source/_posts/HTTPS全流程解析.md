---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWKP4NSE%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIFApd%2FtWi4yUwlw5MOgffKAaK7LnPToSFRowfrDc%2B7ZYAiEAztASdUL8h1Rp%2BUFLqHqdDZRwnYqa8R7wkgPHM07t%2FI8qiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKz1YfXsBjYSn%2BC29SrcAy%2FuosT%2FWavINBmF54LUACJFXHBQlyRcAtXaCA9hOsXrDOwnZORRLAKMsz2RfgzYrKsxkfxXajQDDtK5SO2w80bq7LBlS7GT5aWV2T83twhvy6gAhIAT1YYFLBo2TWmrzQPBpplDPSPYZLvyzBj0JYJG%2BPIU9ZOY6GICefmNT6YhIAbgmJGDsE3SlXKmadQK9GEViB%2B8wjb5Cwq5y9mulcH7zLRBF77oUSaqet4lONf4BSMCyql3%2BYDslb7rJs%2FXplZbcwiAFDEyGIuxMi9jchJG1a%2B7mWJgZpKdIgmVyYlRrBUiUmTcNfBSWuQkjOKsIIFN4I65ArYTyv3fi5%2Bh4i4yPld%2FvBtIE5C3s5oL6wQds%2BIs33RF4QhiDUcoha26MAhf%2BqPt7voB6tz8R4I0m9DkY2tOIgYastHlgj%2BC6bMnB2Xk%2FmB%2Bdv7DblQldoFH0iLaw55NmsnADWPn9X5vwAK55xOMhCki5eHntIgKGegP4XbY2pL7DE0jIEc8lMgd6OQ0IPWrDi%2BelW5KrAKm%2F5TpvOqSGF4uUGHsuTmeUEAVpq%2BT6%2BAvf6lt7Xk40eEilzgg9I%2FtaW1oMZJ8ziwXYbCrVGpWL3msRNLcHOsLkjNpZ6%2FkeS8O8TI1cVrGMOrv5MYGOqUB7tvXISg8St%2FcPjstKJokRsJGDe5OTfYLjUOP1LyIjCh4KVSRTkmczbL%2FchsS32MhtkUN%2BNpDaC03Wn7z%2FYNTwKYKLeVwq1Vyn0BnPKQ3wKvmTKfI7ky7QZUxrcVdTmip93qgIHNpdmPVmbmJZ4R970mkLw5jMFFI0KAE92UziTkyEaD9d2Kx8MQ5dS4gePd2pJOnB3yjlNQk4GWl3rsh3i2Wy3QU&X-Amz-Signature=ca0c78579d44b0de0ab0596c925b6df282c8e08ce34fee40f13b23ee0f544b13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

