---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOQVNYE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKVMwOUQn3Zm6SOCfy2RN%2FwzCK8h46HVYi07yascdFewIgcOZPjV8d%2FfWm6oVgnOn4l2iiuz5Wgi3BbPElLCy6%2BgMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDNNn%2FABtxhOeZzxUGyrcA4LJ1ToErrABMQScaIfbGqEUV%2FEMZR%2F87%2B5Gdq3jApjaXleqy84E1NReN5xKKuhg%2Bj8ZJXmYf8B0mjELRz5QOg%2F7fteT5lFwLXTYHedCbHXZdRpefdQP1t%2FgudJZ3MN%2BmVCxlEHIxEfAGx%2B6Q0OaCM3ZDJI5zm%2BrtYxZPGIgqLaOA1rHEULlBtEBH8ByIQaBRBREiVehOibRdg5xn2NsWNDFRqd0qbfuTGiLFIW84%2F8ourO5Q4g%2B9RNf5p%2FFxoYzfCRWLOWAeeJKz56pgSpu5AVmDjk7RBZxwF5b6G0doeooytIrZHcMPt6UNq4O69jIXXot16Om6Ki9kJPu%2B2c1hW6S88sU%2B%2Fc7kuWXHMZWldBJNDJHGh%2F2jUj8vZf5fRvM7b1%2BnuXHWFRA4iIizXGZ4DaZltO%2FUoB8IU0C5pS3Y%2BQFRl5F1JU%2FAfvFw51GTFqGy64BqklChr73JU3AR0LdOaPaR5Ag9xX7qxNFoqPZ3KwRnufP%2FOIM90v%2FujsnPDoeTINFsBEHSignSKGxV%2BMI1Xej2jQ6rAm1Z8Dav%2F7DKjWzNu1BFtNk%2BYzI2ZBMypeZi9ct%2B8mX9h407dRBDbS%2BXHfpeeY7jv4R2gsVgUOhPWWWK0bOJZb2WOXPemghMK%2BNxcYGOqUB5ni%2BxGK8K3Oz64u4tVTkuaQOYR4tcYKkz%2BluXEEbwTYr5QPSsJa9tE6%2FQgzDC470NJOjpNA4mgu8kjBfpNPDCBF1szB2uMwKREaJVONsEk4hYUa5UZnl8UdO3y5lf672lYF8eiDXBs2EwyI3HmFLp9YTeeCAcXGLU2ml884WD7nNRiVGmy2vWapaEpeBbgwe004%2Bnh7aD2YJpt6a3j4y0dknvEoM&X-Amz-Signature=392dde014756c9ee51a2bac9deb82bb4db459232a44c9b50eb364c44e904b1f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

