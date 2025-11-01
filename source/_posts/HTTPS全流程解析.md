---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCS5OYZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQD6SPWqaXVSPWoXirfRKUmqlw65nk1hYiaT3lN8QCfR5AIhAJtVlZSreHvT0JV4Urx3XnTQBLZOZdk0fNU%2BMTzjPSuFKv8DCC0QABoMNjM3NDIzMTgzODA1IgyhaLcAZf0HbPWatHgq3AN%2BkwJ1QDo5%2BQOAepk2k7cksBuloCKVBqOoALPez0TOobW9g4hm%2FRoymaUcAybxRleoBqbAcKsuhWFFmfgu4kdCw5Nk7OLTBcb%2BQnfdFsTITD%2BfywzjttbGYUwkpKb6Vts7EWE4sXJztITbVgWEbCQoCs4h2V%2BEvNfv0Mpm2hzpWMzJfBIix%2F0DekayjQ4DwoHQYBctSZdNfxUj8pLc0lmLKNRk4kgWa%2Br%2BaqJa7x9tGnMOpNPx8qntEoESlz%2BnyADs392XT579vJZmQxow%2Fo4dCmgTmqWlDRfXyLRrgqM7T2HG91o4yQbO2MYMw9zGpiBomHKXedygRXWLLf7OCMDVq81ffqAX8ORIXwlLxw%2BchJN40%2B0WX9rLzPZOQF1HvM%2Fspgx8WadLzZGyIo3A9WCgNEAGtFNsHJYAd6tLDpZ9TO5hVGhJzEW7VCWB1o5hQdv%2FzLdPdWsCG4M3tYMfY8E%2B20cp1E4x0l%2F4M5NWhoXUnLosY4cYzo7B53E%2Bv9p8SMQOIDryF7gWJrmZ9O0ybVnrvAgMT0riyLdgZM2x6pzuysAQoDMpNdgttdojq5VpF0I4Ft3YnSq2%2FPVPu2ywT%2BjB464aMjEO1gsaP%2FGK7H%2Be58n5aXcXr%2BLYRrEPUzDv8JfIBjqkAS%2FZ9DGb6u%2FuWp70qhoIfbKMc9GeUd40b21s1J8EJ5Nl0drrYHp1RdCVyLz9KSVkt7zjjdTO2%2FewZnl5htVr2T4d9uePTiK3aY0k5K0DQIV1pHTZhoLbUq0i0Az6yhw%2FucSOrMEJB2Ogsn8NmZS2ar%2Bejsr7jH2JkL7s8aCa1rMi%2FS5BEtjy87u0tmYZfBujM%2B5HKVuSbCoO3NRNzJIC5SmzhKic&X-Amz-Signature=c5e6d829dfa22c4591f5d2886b0232d9383cb1d87b41583d3ad7c077ebb9e5fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

