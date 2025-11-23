---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKPAF6EM%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCciAv59mcTxFk5dUv6jJKyJa6jVekr%2B721PN4iskijvgIgS1OknThCM8g79JUtNUohrhstz7U86BHsi7droaz2cfwq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDIg7Ma2s43U8dIGVrSrcA7QCQV4RlKaahyUQyJxB%2B%2B5dyZQ9QpIco9PGGjb59509tC0WD2WIAAHzkc%2BS1r1O0%2BDaAL%2B%2FNFCDn248EI5dRgVI%2Bm0EQB9nRhSYivtpC%2FPoALwFJONadeqIDykh2Y%2FQyWZtJfCzXIfRJpbqcBmUW0zGGWLWmX8zOrGZJ6oNRc7tqh4K1Pbh%2FEs%2Bjjz59QaGH4SJX%2FNRQjQqbC88qXQI1JGVnDl5s1f28zTQQY2pNj%2FsP13B6I0EyP70Hw6f%2BF05FPA0aykxaPBEhF9kzdZXrUMzF7DMHcKIBRYrt04MKdOrAOIblVBhy4PwAZFGUoj6brzII2H%2BkgqWebzf2Mnyve68ym6ARLbQ2PGV22G1jMi4QGcPANAogkifJ%2BJWhTFJSvVS0ykYG48SJsUAzVdkqEZwNOzQUmrstF5p17kcQrG%2FVp3EnfvU1sMVIlaKQVgEURi%2BLX5n34bBvsurKsRgDAF5Z91tMfyW3rHEiV8DQJprSwHA436XXKP3sxPjEPytwrsmtwpe9P3Uj%2BESZkNC2A%2F8ph%2FaMItUVPvLIUVHx6aoxKwLbrWkYCkSPHzXWW3lATgQQnai5PjoVM5gtFMokoFbUBb83T7Zr5jNgH6aVUQu03Y00e1AJ2j8REbSMKOfickGOqUBhNTx9fBerM1B0dPpQarrnYxp9ZmaJU3ufrq1cKMUGvNj14v13STOKpi6EtwwdKs2AaAKRozoBVVsaZoWJ7nLR2R4dDn%2Fubwlrds1bxFoG9uuH%2Fzph2e7PK%2F8SwTalqI8aGNHBSJnqE%2F1ysAfgE3ZyjllhG%2F7879F%2F9H5WlAwpf1h%2FDD4sNGvTkrw%2FEMUZiPQxp2NoZLl6ZA8hECS52rN7qnEjKgx&X-Amz-Signature=7a30d7c30b5faf28495460b9e8abf12fcf2ae24dca849e0fb8e490b54636df4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

