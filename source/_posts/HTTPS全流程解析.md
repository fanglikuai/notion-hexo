---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAC2JUE6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYosZC9DhZlz7bnqjT%2BH%2FXLG4b3QrfYcOjUIfR8jnksAiEA5OJ%2BDtPvRvTsxiRwfP76xE6%2FsN0vzno5wp2hWYBcM7Uq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDOyOapd%2BvZCIv1gS4ircA4g7mpfIioWHzhu4YEJbtvTx2igcHzW%2FksmwJaZ2bo2NgYLr6wo7z7spBWbn72CSWGXh1Rf9Ur%2BNz6AXKFuQFNL1960y8p2Oe8jhXgJpGAG1njnw9LKOeOaeziNFJZHJd3YTZVS8eMWBBMMK0MPh9zylz6%2FcDtjJcBzyEGSE9Dbdcam0fX%2Bth3DgQdVjozkB8SqldtX4gR%2FX4POOe2H3mMEJFXIEneGB6XCgrXY9Gvp4MKfwtLbiyZOk6xI434ZaydCDS6NLChIj1hm86Rx129Nao7z5n83FTYNfYZiSWF3EEyG9fc9XyuLs1QS1x7cfWoYIaiLuWlJgAPHncIfgRu5dwWc1izBdoua27VEcl84zx6P%2FTUsprOBeMJdjWNf4r5zyR2seXzrDRLTCDjpceyVvPtMNinmFt0%2Fzbfp21oJyUsNq6rvBqV6pUzQG3Zhjb5fNd0kax9lPU33skQwuml%2FCncWD%2F2DIZcqwOCJDadmK0KlSOZGjtoSxsoeAVMzl3LOvfGewOcy1ih%2BjFP5yeBOwg8TkZ%2FXAAePYiSVPjc144bozF99o63jG9s7pZ%2F5eT7OigKf43FxGzW6BYD3nbeIn9ySmLqI1H%2FJy02wX8tcJx2buAb9EmU1BVX3CMIGIzsYGOqUBea%2BvgRtcStasRqekygQp6q1kSxaatM92jjyGpvkQrFjDS9h68YtShM8mqnd%2FKp31Yp%2Fbs86GNjE55VGe2YWAvPzp0%2F25vtawpRyJFb%2BcYiCRQu6%2F3KheOKeoP0eO6ayGqjtFKdVXgarthCnkuzW7ogJmZ5J9QvwW%2BTNDHSVQ3e8Vx1XxegKSF%2F3NMxM9WJ8LVGBpSOnD%2BnG3JiH5yMpLY6SzTzE6&X-Amz-Signature=1d06841fbdfc047242ef2afebf7e87e91f6cc96cae828c20050a69e40acb2e7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

