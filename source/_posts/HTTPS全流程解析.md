---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VT4KNQT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGw1iFQiF6b%2BzhdHTOKEvSzBBLGXNVfscpW8Z7f3VlNAIgLdTiitj0QemXNoHakcRlCDWEirCTyHKDAkLJNKU4AVQqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD8oo9pAIvZ8W6zdeCrcA2mUP%2F1L7epmXAAg4t3mNMzJk%2BHIsrA%2FeV%2Bc15seULjGetObY58iuR7MUq6cnsKWiK4mbOqu%2FzYW7naDF%2Bh3nf%2BNsXuuudJI1OukprxE8QEHwF45M%2FH9DmvYBioT%2BRsgjpudRSzdnN0IRszSwQbDD3JGRJyOydc%2FD05v3A0LnZBUe3hztFktjhk1Le%2BhTqQ%2FkG%2BIU913BmkAP0S9%2FNAq%2BtoRoE3bj%2B5v%2FlrM%2FB1voFXaJZvQVGBr6P05zRHBcHOA4X1k%2FPlp%2B%2FbP6YiZ4uprK18ZEK7yEsmRL4%2F4A5BCOezhkynKPuaCP%2FGzryMMrzJfHW7h9vohZrPY441S%2BE1NAnKTIcjbAaO%2FQGxM97Ik14mOKbUWbDPNl2OuMixupepM6n%2Fcci2kPi8JKsKgtdo6IFYxnTNEf8cl1ZeQUvVuiXNPCsxRGUtIxVOzpsKwXm96MU83KUke7ZQea6%2BTmKB1RbGhD1UX851tijvQzyYly54ImLU6Q9grXLxmdNiTK%2BeTwSwftXj%2F5v%2BCmIxjolB953kTKP8ucmp1XTEfe4qC8Drr1XPHOLdaD0vleqfJ9JKrkv34GMNtbJyHLim9jYHL%2BX%2Fjqgl2F3RzRa1vpx4Nl5e3dlQCx422KXEk47WEMP%2BNjccGOqUBpfCwuDsssyxeUhJF8Yr2APhsQBtoRZcV2W0bk2YxwMiIQ6uOMVN8ODHXSjp0B71xqrWPqoS%2B20wk4pG%2BgWJzZOG6gWOR1PDS3hxAuNa1k3927IjQCLQJkiOqEXHFQUEzQsCzaq%2B2ikpAd1G0i9WOqwjoeAYNrU%2Fa4ukawGuBI0oJxrHENNMVhZtf6Td2EhxE5N9q8K1gGJmKimu6sUDM%2FTEnnJ8p&X-Amz-Signature=4920402c9a2ad2ccc8271a8946b1d0aa92aee1201ce39775d21128ffddf8ee29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

