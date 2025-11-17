---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNSEF5J%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA9WfXara5A6LmqfthNyKy%2F7jvkIItipM5%2FsOXI6xa8jAiEA5K2WnQttw7PrNLhou8OR3zE%2FL3cD4ZLIAyYIsQt7DzYqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKyl56YneTmGGJfLircA02PNFJiF6ygNoyvVwPKbihx5C%2F4Xw6Wn2nzrb1Gz0eBpjRrl7JHE0WBl%2BmulzLWyIky02HUnh%2FR0nBYqhW9ix6lwYF3RMArKPDr4QA1V17miljRm8SSAlI2c2FEe0vDIngWKs3WQqZanxI2cWKois9OY9iU2ij5dCMBkZRpXrGJp4c8TI%2FnuA52CYD74wIF9SIYed05G7uS3ASTDrVVsOGzjQD2SVecA58KH1yNtNiK0BZF5d4tNxYuQYWJ0TCoRc6z6WmLZwjmYLgGszLuz5TT7Hv4a8vvfSO9ZToY0NoIdviF2%2FYyKGBf2czXnT1Wnt%2BU6vLSPow6N5fcK5OiC%2FT%2F8zZqbJQ1%2BuLD9u7K7bdJztGP4%2B0MNHJaKHDNHNwcCBijFmjnYDoFq2UGpenjnGjEbt1PrciwRQRbiOaPWvuY58ZTfrrEdhWow3%2B%2BHBgCud4JEotjI%2FPkCuKIl2siJ4tdeRQxyll9ZgLvvy4LUgE5lL1I6hQLCELIRvd0w40Zt66SS%2BlS%2FAziAG%2FPEvbqwE%2FaVjl2ZHqblczNVH7iRwTfLOGrVErY7YchSJ2T%2FSu0zvCCYWalAJ2PKC6RDmh68lrYAlobdlwoPy%2BKIVXEJRyesqSYYn1YEpSLaaSkMMCl7cgGOqUBCZYZOXBDJUDIW%2F8rnLq1M47p21diZyih0YesImLJm51WR5%2BCwc8QXtLt0352YxcZtJuizONuoFQcTzr1U91aJDImk9%2B5jrYsrSXqblTPLxZokR4dz4IIiZ6xXbscJZm0qJ3x3LLaN2AiB39eUvYhN1znYG8jQLUUHAfnv0HS06CL4x2Tz8%2FvVTrmy1VRUMSIclxbngdOaAREbBy6k58F9e%2FskwaS&X-Amz-Signature=2da735141a43f4f340ee3c3801265d111abb274222ea711415376b46cfcaf46c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

