---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWDOTGXQ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCSbWYY7LC5vE%2FKtNTvNbOpP4qyRCPryRlEFDZehXmsAAIgUT5EEwIDAdBDkF4Yy8CVoj00h7R3pp6lgQMAciN%2BYgwqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAQk0EEI0O9sOPBNdyrcA0sgdO%2FNfANd95dAKpHPeNBnZx6C0Hob9N%2Fp%2FYexJxTo5MUhjBA%2BsmVduswCo9DWZFkHlkO1HRvvZs2OZIS6nfJt4q%2FOz6yTPDxi6pCvE6nhrOQEigl9YYv34YoIQQbg7J3Bw9NNG0%2BeopLH9byVdqKu4jSI%2Flg7xuL1YMvI59PsoMwTfhKmLrrp815M7Q6kH3Il5A9DD53rx4kytzzztTLwv%2B5etXoV9WS9t%2ByjXZFw6bX46Vzv%2B3LMX8lfxc0mouihyANPiVqAEXWNvQxpFwj7b%2Bh7Ufg92yhDZ%2FRmGP9%2FMTYrHCWo945VwGDhHL9uVr0Jeb7Gc%2F8Jt3bMZNaWVHGMiY2LXP7bpWBVZlTdhqf7hheDfg65TJUjgByYRAy93kiDNZGYIFJelo1BzLVbWF75bruOdegt%2Bwx4X81CRN8ieK65Clj3yoh6i%2BxUnsB0v9vZbYbxPkqqaHIAdlZUQH08SR%2FkGNpkDQ8BIvqc5srdm0lxjTkItwFFpsho6JP5lB1X3WmBe64SDHtVLEdpBOQIPxbuM5Nr6GsgCNCo0Dki%2FleCEOxIIFMPgG3Ccr8Xj1I6fODf4ulnbNen8bpN5SIW4vUJogCQslD2vaDRPigsTs95K9W7oBsFnozTMLi9rsYGOqUB8dVOSdhDzSrn9p87CsOoQ5WsQ5kQp10PsZ%2FTsi2Fq%2FNMyFphVmJfhv2rNwJ4RhxkjeQbChVAWLmzNYEE8aqLgW19T2whLnpWy2nW%2FFnbIzhIuRIIj33Bl4tGlH%2FBGdXzODy59H7bymeetCmeWHaYud%2FAPCmfGqWBzsZb4TKld6ZAJ10Y%2Bhz00KIGUtz0kVWj3p4NwcpNfoAESTYmLyTpK5GkpDh2&X-Amz-Signature=61dd6fee0bf0824593dbf68ea05f6a069d271bd2e8ec39b712f559d69728008b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

