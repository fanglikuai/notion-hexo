---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXOSBEBI%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDFFPuurj0hy66zeIlOyysBTraVLvcvJxjNDsPSQEwe9QIhAJLuimNqyeLzZU5vTFoGfRvWIa4%2FXb68pH8EZk3CR3whKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzlJc7NNBHmCgMekuEq3AM%2FXpe5J%2BpmcuQxpbJ50aP1yYqa5QvPuQskzByiAVjGRMV6baHdsqsgl0VV0Kc92vRczPhc8Hepcq1%2F3RQSN2NODJlf3cWqsZKQwdQ62V9QV%2Foxy1gCkwEFy%2FPMNj%2B0AZzz4Tnr%2BVp4ngcifLvb1mhufJ4PoKV8yHqLRpIDAS6FOsNfHrKz1gB0oZn3gK%2F%2FguzbHffzmRsbV8MfZ%2FqQyA7HLmI4S8w1cd1pPV%2FHhDSmNks0rSRm6NGj4XBv%2FvEbeVgCAhsPsj%2FM5d7XWU90f12jYboAo3Kdm1BfTW4CFP6uP6YT4SpWABsaMJ7xxmmydHReK0TE8gEngwMeuM%2BObNM5L59FbEPIthpNJ7KH7DDvS1fPDMhwjiiD%2Bu8QA73qSxNiJfrNxxw9fAXRJCucZsI%2B2hJIyYq5unxQSH%2FKECLi5RhcJD7FM18VBbrXXpKllvtzXskeeKax9YyhohUTKzMBkEiBGvLJYq5mh15tfuOdRcLzgQB9s9mSF6QJ7sAe6%2BDG%2BPQWPG%2FxNMNurvZQHBVR%2FM%2FhOAYwC0XfJx336Hpp9Y7jKtFJpv%2FSyfxAm%2BxCM%2BGaA56th02bepCnAsibnszKRCHbavrl1cknjWvv%2FfOEERkmka5ZqJ7guwvZdDCzquHGBjqkAU4lLxJ0RSg7N%2BS8UNrj1sKXIn4KcUYza9W5uCq59TeVHV%2FnjqfTc4%2BKdugorc8igb2zrlXXEU2dS9v%2Bjvn9zRZxmAuVuYh0mVcKZv7KDelmXeKP9TlErgohK7ejUacESx2fHVcAuKx1a%2BYX8%2FYu%2F1E7gjxbPCEx6ZLp4vgop2u78yottodA0Qw5Xik0GhY7NJXleHFlQShndQ2K4itMUpYK9I7T&X-Amz-Signature=a9351d552c9007b6e9b517fd8661d15dc4a52394ee2149fa67b711e29b9f95a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

