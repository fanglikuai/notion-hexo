---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YYB2JFZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDUvNInfEHOZamrFD%2FN%2BjlqYy9zC5kqYpx0L0%2B1uuuZfAiA482YKX9iC8%2FD6YaXH8DoyL47Dc0N6AILWhjkoRrzKgSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMH5QlWlDSh0eU3GkPKtwDZqvFKZCDVbdMEdvFA1%2FrHDvPbgMKcx4FglWayr%2BrCG5hwxElZqorzkHBH5x2spVkZNuTgk9i8OiFSPXV1QGGHgPQ89cke2HXmBxF%2BEAqvTrVp8EGN9poO%2Bfly%2Bu24YmVnA7mqBe2EGMTos1NEUY1agtWujt3Dz6QMcPNBuEk2VPyxmDpcGmmAvwZCTvcsL1Af2he7tj5XdAjFyV5PV4J88cfRlqRHCtdzGc8TlgjiaPu21a0D7WQnly1r2ntEpe8p14OR6wD5ej39l8u0%2Bmv93fDyCz5xMo8gpwmJUM08AWaaxqtMHqyF8KwIB9kltZ2INZ8Z%2B%2FmtbEyec%2F0Y5nOdr77qZP7tRpqDRvJbGISuFMX3QkDjXN5KgWaUBtdx0mRzbvFDevAQFAXDl%2F9baiC1omUgrJXAMkjXZpdBb9ajOAiFOypDT9fjNt4hnEnkN7QzPvoM078t1fr5I6BJo%2B0ENtOz3ipsYtDwwMtyRz%2BmEbj%2FFFHy6Rku9jla%2BQ9I%2B61sRYel01pt%2FfJ8VAFVbL9O7pcqffTotUGmZXsw9O03Cn23S%2Fm2zGgK7HXbSCJVJDos%2FAURlzE0hGiGd4ABo5mfLOqQwWFevAPXNrzV%2FbqsTPH9K07AO4LF5%2Bx3Ycw7JzGxwY6pgGusz4TJSFpLYzccdiNoazb07%2Bz0D6L%2FD48Y5yls5%2BojSb0mXd5vhYi65P%2BAZIWhDdZLklorsBPuOh2iXEpihluqtmkjR7eq4Ow27XaKpnCiSzYjdiVB6MW1X7sbk3eytYaS%2F80UxIn7QqB8uR15P%2BCVh5yCK7O3%2Fw61SotCfezpHrh52xYFxR3OjU%2B6QspQFraV7BPhEezKRBxYvtKBgIT5MjyUfhB&X-Amz-Signature=43fe003d9d081958f721f8feb9f684c430762ace424b0dca38b8e4c3e0d4da62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

