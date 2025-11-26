---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466545WSYR4%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTT%2FpG0u5j8RtOXUAkxQxxk5raiLF5rsZGapnMrfgc%2FQIhAJYBc8YVH6p%2B5rcwxTs7%2FrAECZyAjlx8HEWUnaB%2B0C4XKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgziLzkXX95f8Gayal0q3APxcg7ohm7OhYm9PFGOudPo%2F4K5ucyFs45opBIFWGK2sR9a9d%2FYHdRFnhlhZYCSrSaMsyxtBL16GiF8RRXx41KZbdD4oopwToQbdqqF%2BeLnzo2r0VXY%2BdtND5PwLWsEBSuZBVDc9%2FNRKRRp1uDt5rXmGRe7t3bVzKHdXwc6JYcSw0W4F571kh7642%2BL1Z0jaF5YRXC49RUizihDJ%2BY6Wn5GmoiIwb9FlwRpLUxuBB57SqGG0t%2BRqvSMwi4Eu65I8%2Fa3Y%2Bmg0ntckmeWXwHRASlgNb9Dw0nID%2BMAH6ZOlVjc9%2FxpEUy8AuFvfJn7fpeLfH3%2F%2BCN90g4Bj7N7eWYH2GoMbZgXCHu6bs16FYw32fy8lT%2FycT8beTmna%2B%2FtF%2BLAmbztsQr7y7Tq3cW3ERltUhXE9TMpkL4G622NzDxzeR7MyOo4JLiWxxZVoD%2FxeEAYXE9TWkmUHMuV0eHfO0yfCSAiGPLSl2zfKKXFnn6WP0uulnWLJ2Q4%2Be%2FU%2BwlEMRNGZ6tkmDIelKKKfX4Vyv5EbxVbWqwe%2FHknmWMYMPnwo9KmzcRiHjeDqwqIZTlhB4Alngge%2Bb9njUcZI%2Fvi50XSYCiHrQLrtmtqxXIpA9JQBxMHVEKiOJJ%2BoQcH8Y3AjDDZjpvJBjqkASQpiyfK%2BaFd2gMfgGgIRWyCKAU77zkI0aR%2FCxcwmuLDssy6Jxba1aa%2Fll9KCvAahtNgepokkJHmz38CycL9ifxpzAJ8DtMIh732uLPGIOZx4d4T%2FcivAEbqvR6pajP%2BWA5L1s1rcBy8XusVCtwjfIOULHDMPkbnVyqnjl6TqqOnp0Abr%2FFuUh5oS0iZsYbMG3TI8R%2BU3VLAV15I%2FSrtDXbcQgex&X-Amz-Signature=09f1344e0469507dea7878ded3c1a6a084cc730a9895edb22c0063b58e4c35da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

