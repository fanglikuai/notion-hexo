---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7UW275%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCXcnotzGQXwFAFoYtvOOXVJhZ8cUJF%2F2EjSERtlibGYQIhAM7CRf2QIEeXoYRVaBXloRqi2Ft0%2FK4T%2BWAwE7xfnelMKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp8pJ6mMjYzUV4Dcwq3AMXz0KCknweW4tVKu2ZJOPRvlp4yMHsso4YZcXBC4h%2FJpdR5dlcAxg3MhilEKgs2g6AHGS9HS42lU2jrSDit3UVZKHONmCvA%2B1wF8ik1dv2DK2qcb8jyhXVCHtBYhHUyEKNnnTl8iWENHrWW2nj5vHdyff26Mt472Wwq%2Fi%2F7plL%2FYxIoL6c4zEmgT3%2F%2FL9pAa1eOY%2FqGW%2BfZQv%2BG8x4PrWpJw%2FWRat0QCTtTRIMaMxh3y46q%2FqL17rU%2BrQp4JhK77MUj3tO7fC2F755%2F2NTJB2Dh7yQ8hWstVIPj1Rq57phjvK2xIF6OLTP4KvXVJuFzmqYFSLEoP%2FaBZl7AssUCyxblDAa3CCTxJjFxKWdmuXcL5e07g1MzDn7JGEh9DPH45CACFkl9MOjDk66Zh9M8j%2FlNSdgtTQZF0%2BbgP387iX4vsZOYPE6DbMVXGGF8b042Oa6xKOI6PmKr7VTISHYjJbjvnV%2BKKJp0TKmmJEHkzufs4yM4JaJrpYL0afuo%2BKrnvjGRSPWSakdVjC%2BO4zCuufZupO%2Bmc95yq%2BCFXW978m43wW6VYpF337F2L%2BDIxuaPt7sd1bQ%2FGSw2jq1L4bhZOCgN4QELYZV7YakYNdroETP6xGEepzfP7S6xtPE%2BzDwzuTIBjqkASvdmKNtE9dKgKEKmJwOhl30tT2pKA87LKfIqnlWXhA2sfZoPet9POsTS2aC6QJolmrMKX4wSkkyaBNYJtFi1Yfi8m%2ByD6MqtwyHB7qzvkCh%2FeuJ7Y95p0dpArss1cm%2BeqhpESPnCxcGyG%2BQUbKlmaofCopXFxJtiJ0%2F%2FPq%2BjhoAA2kozc93dpp%2FB7k79jeeQWIpd5%2FKzSLuNiKsohGmRSuu2wCY&X-Amz-Signature=7a92a148344307e52446a69416709796715afd40e045c3445a3cf870bfdf552b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

