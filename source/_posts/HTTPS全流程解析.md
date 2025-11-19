---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THTAUXPA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T070053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGugFLps13TF8uUh6i%2FeTThRmzoWQ2SH1KRQ3llaVNUAAiB4qDVZIOTlVi53POHalll8aAEk95vtT%2B9d70kighs6liqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaZbznCs%2F8D%2F8E%2FS5KtwDPO8A6%2F0mUx9dv4VYt7giWmpojGWqucmtWY7MnpPnRvi%2BTh4smQCZnTG5wW0OZnuj5nJRq7f9Syf5csjYFIfxdVVP3Ofp75tL6w1ti%2BP6q98lHxzbr%2F57qb2ZLmRFyodIPa0RvN5U1WKTAcHKiulPSnGSRf85nZpgnRoIBKnDTmjmbEP5ojCbPA8Bs3iWHLCISBrCkMI%2FDwt%2BhK9dTbKCzoRyxcZSdTs3VEa6hEXIYhNmUNgg1wL7eXWIofbFRD0%2FJbDyMfy2e4xX0T1jPurisY%2BjOjDEW4AfQ2wb2u%2BjdKdG5qPiSOs%2FAxdZUwR0x6XgdyrU6qlu6GaOGgrNEgi6ie8tn7E%2F4QxgsqJ8VV3KCjOzzEPdCTnRo7wdLp8OcewFesi6DABQ0UWwdQPpDslLQX369qoLML23mw90neLoAJIoM0igRsoDQ3zdnyHbIBw1KE8z9wqbFNew9QdFgkJXv7pPHG107n3HdVRHwmrck9%2BPu7lK07bz6Spjfo0f%2B2VW5pAXPMAZN%2B08y%2FskCRc2q43FYNdhAwTNuCVjUlxeJag5%2FL6CLRbbq0%2BdjuEUc6jpoPvMny0x6bMgWxWTmOE5rIl%2BmKkTUUXHmiNzYboTurlkmViu4bJj10jeVP0w%2FNL1yAY6pgEsSQLn8NFaudWgI3HLBtO%2B%2Ft5lizckd%2FQElGW3a6lIBxli57xqMpg5v2k4YWdhUQROHiZkV4TKKldBN%2FXz2g%2FEM6jH4pL6tPw6svGEvPPuJcQGhLdyV2Gz9YmXDV%2BYp58CI5BdVKoawAGwJjd6Z3F1QCEpbCximG9XANzugiF7kSLWhUDnHe2JgI%2BQ2AJ6dBgv1uQqULOadWlIQ5uZBx6YC%2Bt6aORo&X-Amz-Signature=d09333f1939ed6723c0d2cb93f580238f3411cfca41d477fee8d11a66426bb77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

