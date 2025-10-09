---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JDGUL3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T180159Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIB54f2Vd5oKIXXENN3SqTmnZxAeGzcMcfSTVCiuK%2FHYWAiEA7zXCaOoWnr4oYJN6eKx3qOkykz5N6JW7eAsabfj9ivwqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIGapU3aO1nSBqMF5CrcA1WAk0mVn6DkvEu%2B4RjvRq0AUR3hOJQZOnZ12xqr1ZFt2QLWRrHmktHPDZ1OqLcU%2BKTBOaMmWSN1yNLz3fTmGH2ztPiBr2LTxwoC7WtdPih3gHEcY%2FkuX3fhKn6ZK%2FC07%2FBsNWXws5fdflZW9l7WXZnf3L3Ecs7LpgVD6VLJ%2FB6oybm0vf20xaD%2FaAmsg5yU90RC5HhkhCD0DnS%2BfaPscYzC2KQ6jI0owIRzEpDc9GMQSZaxxKGe359cZq4h2yd%2Fq0WoSMdqpyO7K10B6CWtxicbw5WkAItll%2B1fs6lgFBuAdICadzt99miWtdEjBWBRmplvp1%2FwOexCF0%2F7J3OXE%2BZnzA%2FsqpEtF%2FPEXCVFm9m83EwEYTHCPJ1dH4VQ5qsUzEfuDLl%2FFlnw1CJkQbwLCkH1FC30pqkYM30lGMUHNNZOppg8kxCxsman6xAVsnbYqtSv1VH%2FP9RkLuu24U9Zf2266vLlC%2BO%2BbFoTRnWedaGLW4BFrCUUJe4x6w5hMnDEJwaS2vtqIWtrnczjEW%2FkNVhiafvJQnRcFNHy5cr4B1ygXJoZYDRe%2FLy4jnGYqDBoKfnTOjOd0KevDJvgWadWvWqNSux33K0qdhIbSoK2wE8%2Frp8fMg01qM6pEAxiMI3in8cGOqUB7h2kR8Sk4x8d4GiAKPN0hqv2uyJB0Kbb05ks2STuUjNIW1Z8BLcPoxFHgw735BVQLmmcWFL3wIibe%2Fh2ze8lEe%2FkBskFAx4LjwKum0hXtd0WMqBJoA%2BOKLOk6n95JRvULbFyJ1rrQDVOHYyxXxzEBqZVqeqrWcaYb4Bkm5wURJxujocoPA4epp%2B7RgEyPq9YhNlVnyB2ct31whZU4cIk46VCZPgQ&X-Amz-Signature=c02756ef45dba1531e1426f098605598257cfcedd22dcfd1f56c0faef1c8de50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

