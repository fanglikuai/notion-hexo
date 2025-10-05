---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJMQ2IOH%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9zVn%2BEGTDkRin2GaUTOn104UxYsWX4I709Kl3dA4FHwIhALrazpRqspWOxQwzDvFfcoxQ4Ug%2BuKG5hgYMVDkfWc%2BiKv8DCHsQABoMNjM3NDIzMTgzODA1IgwZqVlJLCPbiFiVy6Aq3APvoXOb7GIw5OEllY71sokq7apv%2BtOyNzx6w9YJUBnC7CjI%2BdMk4QXcjuJbQHzbDOoyN3l18lIAgG%2BZqcciLejIy22o1bqe%2BCN%2BLc7zZ41GFPQbMSDwDaVKKZU%2FJufpLCgnAyCwi9BTGAVhnC607rmzMjggKp4Iz%2Fci%2FnBUfdL8SwTy%2FxiauYRRjz6T1oaGkgqhBh9hnbMFCccUSfWJoEeL9F9YPljS3wcAWOo2mc0qLRnHRxArVmyIf5HqCZRua7pcSM9TLwrDIjh%2BMkto3Uylc1QcR44oAz8DNOvXi0DwlY6%2BeI2A%2FymNkKfU5Q8QIrm8YZ85z5jGF7Q7d%2BT%2BMJYCM6v8SLu9ShKCTEeAuCmLUSSh7STIGRwe0WJsVWQJJUT9JOlN%2FqGoY3%2FIRm51r96%2FpA5K5euSOQFrohe5rEyawBdb57dig%2Bknj5yrNOXPHhCLZJXAvpb4bq1ZckHOYR7hP6OvBzM78%2FF37z8984zzOvNVh9Cj4ugPCgJgjI2nMfILcOssNM1BQmWmJfhmNu7r%2BxKK%2Bot%2BLlsBtmVAOewsApvSuG%2F67D1fFzasTyubv8d6CUN0Ewgnc7ejzRs1l7LvN5yd8O3O9A%2BWSjz3P51xvR7S6rkAjYWGc%2F1zEjCM6IrHBjqkAfO0VCeGUSbQ9fSJZKZlTMJ6WH3BILAp5fCfN6axASCUg2ofPLrxKBEDwkYqnbIsiDUII8R4MFmpEO5KFk4%2B%2FiVhEtlhAJdheZo1zv10SWNrZ77Bwb3frmFCLo6IMZsbFcFZIJU%2BzQv4x3Jb6e32Q1VxFIXKvfL1eTlQVhilVd4cGJIDWSGSSm8UxTh0lkrkzmmkltw5uD%2FnweaO5T3d9A0IGVzB&X-Amz-Signature=f3f4a5ddec48e772a9ab70c53fc0aaaadb5dab9c1a2e073321447f21f0098563&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

