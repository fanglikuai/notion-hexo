---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUQV47VV%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIGEEkFtZv1fmkyev3LWU6V4r%2FdmAabXDRUP8nW3U4nyHAiBDgGO19zwaLOSBCeNSUDHyv%2BuiIqIo5SBqv6Daf1KNEyr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMv2o2HOlh33G0qAXFKtwDK6tiClopbl4cr%2BbnydhcUdQ2C%2F1vr8QFowhCx3xp2QjdVV%2B7RZpH69hBB99l61Mzu42oNWa63W3l%2BgGJ7bByU2SsaN2nehmWpm8bgxACDqPQnAj5fNEtM4alJNjzK9NZNNXNVq%2Fi2Krfah9Q5vsDfX7vOW%2FUFCrPmhMX%2B6nMLQGDUi5fwhhNLQHMavtzBvKPSjf3lViwM9NuUFasckU9dJMlkbX2kDF0LpiVLn%2FHiQ6f6lPVYyGsq4N%2FWYfzZ6T5KpKyUgABnSXXhoWC17HZSkIlIJEPseLLPro6eZtnjrpZZuDuNsQpFdY1r4plxaYs7Iyw%2BewAQSeup3GppKI6%2FInFJJiUgD67%2FHp%2FnJD4MSLB733YHgVVhcfuWE35utIvboJyzMhKC9HM2tMDm2u4aaC9zUlidsjVrhZR4InaMwCM0niNj51T7%2BRmJ1mKhvzlk6fAWu%2FcJVu%2BeWcQjULffSaQkTkpjRJW7%2F60W11SUibfdL%2Fe5j%2BuYVMq7BkQMhsikr3OFuKmZckLm8gh3aeD96H9EcndiD1WtvMwq6NyCqJ1bXiuY5BMHiBVBg%2BNom4QCwuLt6n4rSE0JXdfT3hhp5B2PoIYx1dkFZjvacdZj5%2Fh7zT8JCW4phW0OEgw%2BtSbyAY6pgE2RSahduQo%2BvSKCZSeewPiikaq7oEX0hoCixDjGrD4JBKPwXiGoyhI1RGAIAGAp6jTiuHm2jWV2Rpa93i2%2BDYpq38K%2FQO3kwgplqqM3zALl%2B7GKXPIixUZMdyxDJ9EjGInWW3zYtAS42f9JxnwelY3BtukLzmTRocrhlnwJ3PEUkt9EBgMBTH36bMdLzWLccL0w7m4I6NoYSiBOEAfvfc%2Fob%2FjhT2s&X-Amz-Signature=911deb6f078e8c544c09ee0f3a4dc302f8b7f0c04dab90a00eae0c23cb085286&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

