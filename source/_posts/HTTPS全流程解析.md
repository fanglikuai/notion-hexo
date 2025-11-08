---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KZIM24W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T140056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQC2cVIuLw%2FQVsp7JMG6OGB13H3BKgK3hJD%2FFopL5K2EogIhAJoCyfs8AE72cEyW%2B1YdouBptzY9q1UAIIMOqEzlh1JtKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxfPXI69cZBDPSabpEq3AOWGM2iEUzL36rSD9jqpvk7RG94y9uFdvsLxMsCI3ot8g4psLBaQ%2BUrFBeAxpj%2FmXgSXkK9nNWX9EQU9sIjsqh8q9%2FU5MMz0JGk8vovFGWCIiVCqN5v1dGbxsTY7pO7ChJC04wkVif6uwMshGyWavcREUNmEsowsAFBk5plildcj%2B2v1AKRsnSIWfleF0MAtrn8SBq3aMVRE0TDspA2uPJJvITQ4dBcUQxOqz87BDGW3gsq3cVtZvF7Oekvwmageur1ak5BEPAhh8s6rlypZKiMLumOvocL%2B4BnK2gV3Dml0V6rHyh6AWhiLGIqkfBYbzc%2FbUf69%2BdaSDZN0tI89Sv%2FL0mBzZDoubNmi53qgKZ62iX9nGpl%2BGxD6ijPlSCVTlA%2B0sdEWOeYi4s%2FuKdfPOSlrXJMdqrLnExPA0nJMx4xX1rxEMyck%2FxLM3DMQoZenMxC2tDUT0Rd3fxaou%2BrPvzitkfswMF3CNrir8IXHeHDgrkeWPBXNKUwyYonoSXca92TnI%2BhPp%2Fn1ghntusyqVvRgHB8o34NWs86XcAj8aOUwCAyxOFC%2Bau01GbADrwx1IaJ9%2BvCbAqck9MeNKAYrsmJ6%2BoXvEJnmW5ntzNg8R%2B1K%2B1cMTdAbTowPYWePTC0jbzIBjqkAfV5MBcBo81yMYNtbB849J8ctYBF0ojeGqcXv4grzuVGW0A%2BgqWY5w9PvsmDK5YsLhfSMVuEtMq9ZP3tOP48N4Bl6TS4IRIniGx9QOHc0seeszs%2B4UnTbE3vRApr2iFJXu8fk9iRLLrxY2e%2BNmC9sUJH%2BbBpX14sy9Sv%2BHJCaKG%2Fuod1GIjyyp3aJuKAsnr5J5CyqycDa1%2BtDTex4FTRDzlwV6GJ&X-Amz-Signature=c33c4aa80e412f112b8a2e909875f5d987c66577760481cb6e5783e7a2e3ebee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

