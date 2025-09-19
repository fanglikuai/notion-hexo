---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMJ6A5T2%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T230049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQC8w6c1iGEyG7X21cF9YBgQqt32iclBUgpyCVmHFWdgkgIhAMaz%2FXG5e4sSORNRMXJxRe5zvP0FuVR2md%2F1yyqzJNbpKogECOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxaj1SScgkkj5qAMYYq3AN%2BniraGo%2F%2BlzQGaW9%2B%2BmT96U3yTJxhklGJ5X261QJUcEzdmbu1FDoewEL92CdJK4yYuP%2BgYBbwGu%2F1lE%2FXWh%2BPHoi1fwL4%2FJdXa7gUVUDMqcV6VZvW1W9N1XDAHPmawCpiXn4A9z09AsbwmuCqyRks175xwG48u11n%2FDfCGqhk2ToDO%2FXE6oIyLTG7RvOm8rGjhRhWenOj0vtK1nqg06BfTvoSLdWdQg7H51rFAzFyyOmunK6873PC0tI5U1JvSuNhpKfstv6ncNHCH5%2BWpsStAM9PWW1wK1c0oAo1pEsgYYEVOIEBqHk2eB3%2FjHEhPHmdXKf7b6mvS3HZZ36oekVabrwPmE8tyl1BPiKI7S7e72bcBKkM3W78%2FGv8EtHJtJL2jQ6fTJzHeNjZs4tuYl0PeyDW4H%2BK62uUvxUUYIm0RuZ8icbVa9EQjZc%2BpfIz8fz%2B6p9jDCILGVLwCMyRgP3oGSqaUmDpui06enIIvoYKbKNINVb%2FauIY8Z3SMbg7aEaxxyGMCIzlPgU1dK%2BDIbm4WzQyfSD1EihhoDhYbYYKAxXBVuyfcV05kP4uWoR%2BZwY7vN4czbWCeMq%2BPC9YGZcHAE8lysi05%2FNy1EmRbEJD51fJKYgWQIgIxfCxPDDNr7fGBjqkAXrf5DmHsk7XoI49WMb8WaXWqECpXxKzFQwZ6MK6xIJncfZcpStdPWIzKLElb9hWtuS75%2BgvNrLvK03RJoS9Gum8J9pGLzzvxdopJYrKb9DwJMntYRg%2BJOJq235hOqVTmTvGfW%2FVBYRV1BnAMk05IbSU2cs3r7h%2BG9KR47pXyfAq4lGnbleCUXSo2fkoZmtV%2B6hGjPl5%2FnKElL7cg5v%2BxT3KXus1&X-Amz-Signature=47057cb581572479ffb9bfa05e8c8896543cf7447e52d26e58f9be60f8e305be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

