---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OWVBUEF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDxrjI4KHFoufAxRaSWE8aPEw4gmU%2BIux2d%2B%2BkOxSKUbgIgGWgzruVvChakIr23cTuAwK8nYnnp%2Fn%2BjGjsTkbK8U%2BwqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxAeZhnN3q5IkZ%2FJSrcAwmkhDkCXJz7IEPTgKxnM6ilFw4BYmGR%2FLlu1NvJFL6UZFBxpDSSZlkLP26qNS%2FQj%2Bq1Y13o%2FkJdAuqAm6OHKTQgJAx0zXbL6dNd6zNJ9pQOehXJ6OyHeipX9KbZZL8%2F9%2Fb6gZchMF4zJUfMfM12dghxxTBU%2BdCgBhkzcxfUnMwNSlPl0yL6HnnbngFlxseLznyY1q9pXdL6ORLKzXMo2wRykG8FxJkrOVvAXHQmI7A6%2FWt7f5K%2BBC20m2iwnIKT20zpKZ6dACxXiFFRDpozjDGmc4wBGwD6vfBrLKVR1B%2BOdZTErIJlfLM2VkL7YeFHzH1oSDu2W61TIAP7WXV37PRLuLe0pd4vu7mtGmSrVpoCHv0Q2HO0AKYlrEwj2NARk9qAUtwEivCh1r%2FiY06Hfko8jKwm8FQVkBBFSgBPpnBYNsPME9JNyy4fa7FF%2B%2BvskMjUKxBKt6LsSV%2B9demeyYGSPx%2FKxmUQhxi2NCXlH6uf%2F9Z%2Bhp2VFhNmMmI9ieffOvpPv7ynx1%2BhoiCklRjdFaYSWSKRKqlJ%2B3tGuZP6z4cxg4uMBPjK9WZNZ2jCC%2FDF7%2BTYkVrb%2BBfkqhXtzruHDBVfbO0i6X2WNORlTqEX05V5uzUBNeVm%2Bl4GyBv7MMf68MYGOqUBdBs1RszB6AqZS6Yle%2FOrBq1mPh4Bf%2Frj6TkZWl%2B09Smu%2BG24mC8N6xnpWT8pAO9DTMaFiSyP6EOnv0kwLaP65D4NlCmCVCyNqF%2FIkEZxXGeWI1iTPXOKIqgSBF5Tr%2FlDmoYUh9KCvD1pBZTwEn5frgN4GtWwaBo89HmON0QMIEICkwVBcTHyiOfn6q4iXjXvFkhpODs2Nd35Vku9frRE8LM6oPfg&X-Amz-Signature=f58afe1f18ec099feb6dc2b2a1e381c5bf238b69fcfa9b9674dc8deb98b99e99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

