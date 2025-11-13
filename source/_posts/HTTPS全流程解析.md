---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKBKQJD5%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIQDotwumjP6BDJzxMN6aDH5JLgr9iHZZ68Vc2sT727PlUAIgIEQiV7u%2B8Y363Zl3ZpWtRpeRN4faACylzLhvB1yzqFgq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDLb8sGbCVOJdXTUzwircA2iCiqCNgXhmBDwnDrJdr%2Fnz5e0VFbHDqB9%2FB06E2vjwH14hiu3GHQcgVPz7w0UG%2BWG8siwkqMJj6nVy9iONdJNW4J4oRw4%2BNi7R%2BrIto5jv%2FPlUXCzynKI%2BxwdvNbyafwSjWrQ3EeoVst0d%2FGkBiDuHLxJZNzzF4WFm94fg5biNX8oMyUq%2BdZEqS0Z8UFPCHLIcOfQRHj%2Bq1JXT0%2BF9XeiHtf63RHwdWl8c8FpWL8vi5p4Kr25rqkd85JSn3TrbrfkQzLv4YxXsJstBZY7xjBJDC4bGzxGmcGs5vG%2FPiCd1AM1elSzFrdWwbADmvIFWjSNcotmnxxsjvHmnGLYEzTA021kTIawdQCwZyQ8W5Dc7Zv9TyCUZKZIo9cUUtOrKB567E4OjG5n8Fig6YoWhIV06FxkZitKaFb2D95s8pcG2P7%2F3klwt71czvJKTAu6xSmn771yLByoJwwUUxB%2FWphv02ElYlJdiRyz1m3Wjz6WB6ZpBNObBg%2BvoToDRYxlDaVO5qQZX%2FfY1Mz0T7DaWl25KA5xBBumy99uZN6bLV%2FNcIvrgbxGgEA48pBQx1Z3igY0Tn6cusb2eE6u0b9%2Bw6DxJ4FglynaJi%2BOHJmhSnJCSawiw%2FvJVKMWM2xLEMK3m1cgGOqUByGiwLxtkgwvdLENT1dFQOAxo%2FFX4YoQnYRW95sw6yFZPgJJoJP5Z5NHRs0eGyQgfPgWSlGuc1eWHOxBcrAiEfniVAGz7twdA%2BO8marsH%2Fwnjncs69kLCdLnLKz8qOfSqEcRLKywA9CwGrS87vFoQB7G9lGxXXyMSuZP8MCGIyzW3s6LrStKq8DhF8xbyP%2BVzo%2FguzLUznR9lSkz0P8fRK5PI0ORG&X-Amz-Signature=5a66d1afd08edb5205dc3399853bb3f1fd5e42f88d1dd478a7fd862c1985c8df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

