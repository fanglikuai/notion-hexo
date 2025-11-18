---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664I45ORPH%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhprBeyNA%2Bbv2319r%2FaPL6pR0ee8%2BnTx33dlP%2BNv2JpgIhAKqnwTKJ91MXVYpdksEfH8jgnZZqMzmU%2BTe5BIPCcZNcKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMk%2Bij14iOCcs9c8cq3AMmMZug4VRimcfRW4egtFz%2BjJx6a0mkYrKIcnmIG9to8vKpMBR22w2vEdxXHav9gcvP%2B1lDNpdnyIpokrVjh6z3IQeYh8weqElHE1k07%2Bbj1rX%2Fc72XpEqQ1lrcFTbjfUCD688qe0ppDN4s7ABh5dJRfTgW3Gp%2BpLygCMrrGbhf%2BJMUBlIodPcDmQLiN09Fiu9aqFWInv9jGlG%2B%2FuasqO3WjiI%2Bl5p8BjpMf5OQHzzHAoQam3BmUUvW9v8fpXGEJeYJOXiZbu%2FgLEiZ73Oaitatc1xjWd4rVB%2FiUlt%2B4tigb%2BjT8s6Bx6OZIL6t2AU5Q0%2FELDbCM0iiIh9%2Blx9e1WlbsupBOe79k%2FZvcKZ53XdRpNQ8UQLO7UcI6%2FfFMEk7CKgyu6O05j0EBm%2FKUADEzmOOJAkGozfFevcDvnK7m5ET6VErYfNE2GjXFL2swu4wceS%2FmB3S3My%2FcSqxzu%2FkN3DGTO0xjG3%2FlP13Aoo4g796iMauA0dVlwgCmqg11mBedANmGPZYaHcn6htkZD2sbqgsXNS%2B6G1coFP%2FRNyx66tffF1Z5N85LSYruvb7ZL5F0TM7o5i%2BfnxYemuVY2jejnNH1vZHGHCJ0tgMN7Z69%2FBHNFYJ4PRQ6FQ9rNIAwjCOxfHIBjqkAXk8IVqfh4hAOsGIRDsDT5AAnM5bFv3gkrqXyzOPwJ0X6AOP4hJ11J2eAlrSeakXNq68FKN%2FFWxy%2Bkn2EbeWp9Q%2FSTmFLJlz8ZfJzG7qH4FxBixoSJs%2FqSQQenMBIVCqT%2FmxOb7TDTqkOSt90sC9s3k0o00WLruNlMCe%2BupnRfg3e7v1a9SKiTq%2BJ3rBVf1L1CAOQIh14yNQ7MMxZdU4hfmRLoy2&X-Amz-Signature=c744e8226ef9980aad5d2adad8166065a47fb30c4e5541b9e8679c21ee3a4e34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

