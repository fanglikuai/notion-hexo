---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFSMYXL6%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIQC3CV3BU7bVGTuDi6RwAoGQfWnLdALt7N72m5kEv3hJgQIgVO7wWB6wKInPmDu2YXjtrgff%2BKp4cZoUZTk6NSQ9tiwq%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDGrowzDryHNnsKHeVSrcA%2Bajd%2Fw5tS3P4g%2FiMiBS5iKtC9nDi6%2FgsfJaIPg9joYXJW3xthIb%2FRFY%2B932Z58MhWUm29mkcLwObriAMpuxLMTJrZFkhxvX2WAbWQ%2BqeQtmeBF9s%2BWwOTthRLDFGPUbHw7Ot%2BjRxJsWSUoOF%2FXq9UDtVuVP9ljJFWqtdryy3pdjllYnZrPsi1pl9bZ%2BtqHMFTHmP1jhcGeKDg4R29X0yrjd8A3ce7cLABIbUZEdrsI9ObnUyz7oAbTsjIECDK5JOr5Vw7FxR1gjniyh1yfypw6ogsIqOISN7ihUdDa9VzWqoGZZn90yy4nPsPKrV4U0k5CSDke%2Bv1I8ub4gmjwhlR%2FYPdP3EKbG1UwkuDhC46IibS1dEg0t70a0rHqH7skTMSff91a%2B0I0fU4pQW5zRXbE%2FykR94tjg3QsEdaIRQq9Uw75S53MPey%2FxhEdepu8F30qEPtyk24TbrWvFb8zdnb6ALuAnzUhQp1liGtwmFUStexn1OX5Q6L1NJCO5OtNnmlW2T2SjSixWyIGNxnDgxcr4kKl%2Fu004bI%2BncBA6RBvIWzJ2UTE6wa3t4IYAlteTVb5Upne8dAyYEJdURVFdX5x2ZNP5NJgsSdrfT5mHq6Yw9sjJjKXtXW0PhIC6MI2Mx8gGOqUBRzbzC0iEmMOHAcu6w8Il%2FLmniA8jNkW%2FI4jX54oc9fR4Ujfiy57o3r9u%2F2Z41ufmyA1B9PeK71XON1fHpmgmkQFuJJ%2Fxq5dFi0ywh3hFX4GVskCyerVMR7HVuIaNAk8gN0zv0ctDFvOyAa3VVQIgBAGgL8kb%2FtRK1T4cDbGoOHkUcyZ0dkkuNOyB2TExf%2BqLH2pqWVisXlUGRdlp4R%2FE%2Bor1KsXF&X-Amz-Signature=c347f1b0706793881ec469fd15161a33d4a352445ca79212ddb311e03d5b3603&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

