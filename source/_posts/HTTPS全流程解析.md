---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SFUPPID%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE51cv91Bs2CPbPNpka1SQex7xJQRrmDz78xJtKQpekTAiEA9EiGW0xepoOjPAj3Joth2AUjrj2fInMzVwfS3af90hIq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDMJck%2BOVjunfDFhw4ircA0wQYsJJPYBDn%2F9h0MG7cVqRnvcXx833wCN4XAun5fQJqadcDXFmnf%2FtPFpnTefEhh4%2B33KzCdSaQ5OKEhoNSzjltqDbgir8643E2E4v4NnrPghPBTRBk7YjhLvcvltNDcc9B6Ilm0kV2Ytgbt%2B3z3V1RxPzLsr2ltrkwvKlmr0EhuB7PhfKiUnBdTQY49PlAafr8M%2BD75km8bZ8wSrVStg2QGyIAH5qdOQmKwPAqtj8s3rRbuGjsciIaVwZIiGf9c6hKQpCv0dx8NbM792T8Zw0qi6x3LEglq9T15nvitgTXZozNuR%2FaPDLCX8sJtd7bbgjPuBBpOjlfLkpv7gcKcdFQ4HkSKnDvLf5UuzeB9A3Bp9DK8V1QuEt2P1JzEmKhfNBQL0e7T6Csw08%2B13NiP0qgleU2FTmryDyjUxD8VTYdtE1tij12JdJ7Hhh7nkxZs4KbF8V8Ols6HOEThoYVV4Rr3smPw8K9%2BaLrNNjBKb9u0MUp0IoBHc39YZXo%2FuCgCLUmTi4%2Fe2Tfc3rXfrdaDo2N5B2Ie9pZCPggkpPZ6POGttiVXkrwPGW5GLZUNxax8vwpv8U763Ds%2FWxG%2BZkGVd6TuGe1vEjBtNnZfOo85FAf2yqP%2Bh7gFhervWYMMaTysYGOqUBKKER%2F0QO9S8a8H2LvJsgLNM%2FofxX4R41Kno%2FlsQkWGDRcFOpmmKs%2BxyNSs7nhpDgUEX8H%2BxFcgicQu%2BjySZYmcTHMwqG4oHaqGG2hIfQPn6cpmZRz9%2BQ9kGc1ZMV9VxnGIwSvwDzpKpzWFmB%2FzneJH%2B%2BcTSqmirA%2F94eakqojdV2Cw6HMUgbLtXY%2FGRw9CEKwFKPmuOhkTMu2LNgg7omsYKXwoVa&X-Amz-Signature=290fc687d5af82efb049545ccd3cae8fbba40954c5b66f3cbd1b723e2a67a014&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

