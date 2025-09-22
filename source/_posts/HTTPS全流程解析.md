---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URDY7CS5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFeffvxpK2QKNFZtiR%2B2mgeV6hP3zHUg8uoi%2B1bSFBnIAiA4tMange1vnqkIpzG9jrTh2t%2Ffh%2F2gU9ljii1KRWi%2FKCr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMN6AIR2ZNDdNN%2FO1jKtwDVOOiM5h540IAoAkYN1vCC6uueU8ENAAkW%2FnEBK0KmcFbnCSNHVelxhEzCK8IdzZukf3HPQykHx6jWNa8PJj1ywD5%2FvNXHbL1fOJLjIpBnOEO6fdBohX%2BNBLOIiler0K7wd%2Fwv4aGoWFb9D7X9uqtpQr3NoHpr00GGCpo%2FNKOKupVm3%2BvGGCcz4iki018VJMrL97ZtxHR9z73cF9YF3IO5RC6fJypguGqNxgeqHRzge6BClRf5vrTmePgAJHCc1ZsrH6wQ3TBrZo8rN2NPVc92ZJDr1SLPEXkExt%2B7ucLkEC4yBgV2CG2jN5W3dMgV1%2Be6I7h2UoF%2BBJG7FXGJXNWGAxfVBf%2B%2FwzfX5nn%2B6eaHtFBjF83Va3SFjZp2G0lB7peabtnG00mMBWvMyCpsFaGDId7QMXPKOH1vK4wDuG1NQBH1Mr0GM0Xqwt5wsOwZgT6a0avHX1ZhIGzwGZyIIyk7m06zppn297prmpJNExSC994fdJF89YXwfL8wp2UldwGlFeWC%2BIls1I4zRw0yJch%2FdbLFo%2B5SEayQ5Yc3ArEJ03iaLukVHDT6ACYXyro1X8dylz%2FAcyzEOcf6mDaP7cvOvVjeEsMDyNr5xTUE1L1KKw7yeBf8Q9A20cKEscwg8DFxgY6pgGIVL%2F8LM1VBX3ccgonCIfLhRUzPO3WjEoelNDoNm6%2B3Faeov3TN04EaxfMQXG2jTZ03UR1vRYQj6zHhsJSCTWssoz4FdeOjMQNUYQWBAEgeB5zIAcK%2BX20kuJIBCRpkG6313V9RU6QTfiYe5hFKbb%2FaSWig5uejGOnCjVGbZxhgG4%2FQilp%2FFd8hPsXgVM20ewMS%2FG1yk65TRZF8wOC4JA3Pro%2BVCd5&X-Amz-Signature=a54d7c862f49893077aa67d254a72c80aff7783ac8d998388d7d1323cdfb6222&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

