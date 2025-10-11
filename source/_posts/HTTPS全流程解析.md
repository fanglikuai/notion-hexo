---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMSQWIDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDtftWTgUGJA8PY57CVOscx%2FTPeMv7u2smX74hlywN%2BrQIgPmVjyIGtTI4n%2BCP%2FQ%2BZI200zOHp7lmmLkWZl6986sksq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE8U%2FTX8zJFZTNjHeSrcA1SmpUzzAAz76hUTQ1wLw4YLWus1pe%2FLDHuJ70U%2BhCnFswzRsmi4lD2sj9X8PY1uob0Cp289yYY2gIp1C2PV67G%2FACQza%2FlPYY1zn9pLMcp%2FU5sy6hyMZ6F5UU4rateD%2FXKsM8VEXmfL%2B8wk4zobioSkOo6wJ%2F18TEUImvLRs5fjIv3N43iT4aOJ2jREnEVuk9qbZ7z2CWJZye37xJ64wY%2FfEZ6eu5tCv7SmiwPxdGzRPyWrcrtRzWwC7XEHNnZym4JxLQTzBB9ua4MAIJomiWv9XBWzuHwkOr0l8ECEEEjzFGH9dRAf5BcIIkC7RmUGdcaa%2FCbt91jGNrRFIngMJ5AWeFd5cobqWsA2cbxxN3Z3CLePcVRj%2BrKYiKA7veJERnbDLgLViVXDIp6xK5yG%2B2AxnUef9j4VsIdNUCFAHO%2BXldlinvVZnkzO1uXIhgXgSdB13L4l3BojHm2Lr7XzgcFy4RfAqnFrki7%2FmVXnNHnDBsaKWxQJZUCxm8TkOsuPaKSlTTZ3qQaw16YPhv1ZicdQyViRiFsRuc1xIx1gHQ2G3tgRR5TGkR0sfo1ge%2BrfQ8Cf4mduGMKy9%2BimmpzD%2BdkGWuOkqx68Z0Yp0aMMJni2PJHzr0AhwCyyQ%2F8XMNGkqccGOqUBtprjTd5zks%2FRsQhxDduAUWKSHIDg%2F%2FUm0SZH5VqcdAhABY9rGgg06H5bgx0dyHWBJbCYv4sFml5fpqmJWEUmcsCDPlTfMmyRLk%2FkoHRSl612e%2BMi2u6YEgdOkGrFhoZh5F2QGvJouUntsyzXpoqm%2FBq6%2FHjLoTLl0Ph02IjPRteprR3vonyJuZ94WebYbJwJ%2BTmdep1orQgju%2BHsmECMATjyELeF&X-Amz-Signature=fe0656b76c437205806dbeb088889f0b3b803e4009f981e790b3bfc8138e6e4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

