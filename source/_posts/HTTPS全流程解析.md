---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NA3RVHL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T150116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAd3uPSxm5%2BlBMKWA31XwcuEDOjeWuB7B7JuqsFsGlc4AiBNrXiasKHnEBfoqtwErswxTBeltLA8g%2FFKErJ3Y8n41Sr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMwR3Dydf%2Bu6G3d3kkKtwDb78lFoXRfJ86I6ibn8HL2UczAAfgh5UZj2ZazXcCc17LF5lEnvApYvuxsHTO4w%2FjpXCcN1Evm9kSF29eq%2FTQ0HOuYIzqu8urIPRuIjlSUzLcSeErXS%2BSsiNYIduE5KgL0RlVWGxnrm2CXSQzRCaVFOuh2LjiH%2FJLeRLTPhWk3mFC2aIjVLADLOlr2u9HBZLCy%2BK6UX5iH%2F%2Bkp6phXvwNIUW6eu8Tn%2BfNE7xbRulW62nrgO27x4YA5UOf5U6dwFf0LwR5%2BkZ5GVzKJlod%2BC%2FAciX%2F0FeGgublAC%2FayG2lFZ%2Fzd0QU%2FnFCMHXtUo%2Fhr6%2BimBAJJa3Wp9J4EAoGOlPHgCnrbI1SWZ5rshEmG0Mqm38k0YQcLbc8zki6HtUEpUS4K1DokwR8FQROAHBgDbctlHbLKGEC1kaaVslPVoJfWoV2yngIkUj26iqeMwaRymWcnwi14hOigG3u3nDErei8HdiFlU6lwCV0pj%2B9JWkbylJ4b7c%2FEXS5gLzD72sFf7Bypyfbw6rHV5%2F2aZoSBjCh8D5pUTizmxYb8MOMjih9bHDQiT36OPxq%2Fk5zZI%2B%2B5fp3J3OPxe2fbttbJp10xP63gtLJPh8jpUgh1zI6eBorZghVkB%2Fymq5ZP0s8I6owlu%2BiyAY6pgGd5PmoXdBBUproJOvOV%2FK80lvwNllebuD%2BynO1Y3xo7N0S%2Fdx41iEhDXM0oIcfxK8BbnquD7wOfKnrZV%2FaRyvebjTXb%2FpD1BBswx2iGCB%2Bf4q7fS%2BeISuwjO%2F%2FfiR8pZgRG6Tu8FsaZuZ%2BCQacr8ARtTvscS9yrYUtB18oX9FKvaUnbyRDN5tbnVF0U%2BiI5sJDQyXo9HcweZ0PjROeWSBKirIai9dk&X-Amz-Signature=2dcc602e68160c437e5841729e2ced8a1c7aff57e580c62831463026089c5bd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

