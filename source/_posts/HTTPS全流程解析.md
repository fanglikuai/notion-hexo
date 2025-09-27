---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TFG4LAJ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIB0J1JyBk1yT1h8DUc921bjBxnMsLcdhLPR4QOnywYVAAiEA5WjW3S2OWsVwP6PAXpZB53LJHDNjkwmg7gNHRdK6qWIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBG7xPE573zwhIk3OircAxDrjqpos%2Fz9rlFM%2FDGv4R38W4VXdVuCfXhLIt%2FGg8GbTLEl%2B2FP8lsJ3cvL2NqNRU0z%2FJOdagD%2BZ8c%2Bgo07DXrsUXWvflBstVQ0dG6Ljb1jCSXVsPkffCrvkkmWmTgEqoypIK%2FixOgp27VrWVRr5oGDC9xUFNJ3EMndRVOLC25VYQyCEv4lptricSjfekrpp7100tRF1I8Gi0QpbnO1emSkDSeKqiBq3yj%2B38e6ilQlSRSmo6jtPTOhJNksSTvwxx2Y5YcWPInq2mZXx1DtMdgX68lxaPZetU1yEswYTf%2F5PfGKiNYUEQ6%2F8%2Br0JXezvLMAPINciikTADDvpNVnjA32vDs%2F8MAq01%2BLDt%2F9h%2F9vcU99Pw2mNNRP8%2B%2FBMaZRJJwBUfNIErxr4ifJhDhKarh1422eoQbJYBcq6gl694%2FLKwBsPCxV3i7qQcn7IZAMrpaymjWVArXLcMkT53PXvZTA8keaDp58HzvMdNbFQ4KaykM93AFBUcuxqrK7tO0l2lC2JSjfXO%2BMFEXrJATshoL1xV%2FNbAOsWIfavqMXekRwUbPtYOrV4jvkAqe9DU83RQ5gxPfbQ0y8Ubojqn7Jvr%2Fk9uqYADaUAWmFwnqC%2BDJm7yN2TCGlfY7me60WMLHc3cYGOqUBWjLVvShKcfGtDWBMIGG7s7LhNkYWybb0nojnnDd8ziWbYENtPou1nJnijOWf%2FnG3%2B%2FyPVGfwZ%2Fv6oAq92JyF2qAfRCqJQzpW9AWkK8UtolDnyIb6uUUIJXbqebaKRT5WAbhnZHcnuUX9G6tCg8yLphgEL4w1N6LA0GPDlnO%2BDy%2FowuZm3TGFLp40eCMg3usOq5Yco8LG5QuP0PZqYpcf4JbrZcpl&X-Amz-Signature=0e777b3d1d024463de2b696bc6ea5d8d2a392bc2e269a2ea9c8dd6034da2a7ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

