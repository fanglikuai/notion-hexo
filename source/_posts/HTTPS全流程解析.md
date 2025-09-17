---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666U3GXYHX%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCICHOh%2FymvsD9FVx1wL8wsEbJ%2FHXH%2BTGvuD8qn6I3G6vUAiBu%2BmtIFRDGggiSvC8nHRWDaMRtZs6mFfUA5R8gC2BFViqIBAic%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGJYXiHjuEmB3Mb04KtwDzQSANnLiPo%2BCNJcaQfkJma0Db73EWFsiO3fnPYnu%2B5bf7mW7SJxVqjPAXOsITyIGGyCL86p6BF385mGLVxUySgQYVzLyIaGLZRHUs3N%2B7lFKVF7OePUbcw1l7Vqq1Xssg9c2hLVQI4QwrdBBqpDIHkwGMiIbOjgV8WZn3g2cjL6KQvGa%2FYO0g7CHzHDO1S6q8aoaqKjgOs2MIaUpU0o%2FtgAYjgnPPQRhGG2h1LrlgfMAptQef3XkNGOpVpL1qaLrW7CoomGWRvTHQgXOgeDw0LxWXNUP81clJpsQFZZhCnDpwl8PG2loz82HrXl3j92w77KCTCdUmro%2FRv9%2Fcjofb7ww2flYLB4pKu9GMoUhedWyYemh3d3PwjqijNL36cIbIuBwUeiN8rTOtMgi7cXJ44MiIy1GbpBPBVz1qY5U8E43%2FUPIwC%2FrzCFQnlPRPSwLA2wsZXmiCfM37DNwCP1V%2BlAZE9Rw%2Fbvectt57g6aOuuUzlBxsPLdRpCQ6ePJVub%2BsiH12Aqrp%2FCTruriMedUTRuPtUGQiL%2BPE9c6VeKkCIBo2FCtCaPJkNTh7J9Qxbl4v7R%2FELAFspensxZCDX2lLwwvIVx%2BMZxRXdEZ4rq7Qe5FUWgATpIlt11rrb0wgdKoxgY6pgHFRw3SPoZ5W0bidnYYu8aEbZ2g3y3Vbcl5%2BuUxgPZxJbJTgVX7gm1g2FSnPVzwk3wPSG8yrFyRjM%2B39djr2jjchfiBbAOOwe1GJS5iNd5nuOl8TyxjVKGUX9HXZ0K2k0HOLLRYtWRyJ1LgCtDbJPWF6ZhS7wRdW0miieBpNYut8Kbf2e%2BHT9H9W2Ilpjg0AwEetGPTe68jY4F9xfj%2F47wM2Q2ChU0n&X-Amz-Signature=18a078ad95a48ecb79236118783974bedbf45d5381c83c755caa9222fb6a7df9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

