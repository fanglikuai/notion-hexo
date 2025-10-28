---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYXFGVZR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD6x2vX0sMFFpt5o66BVB%2BYC%2BoNrJ%2Fj7bRza6LpK1qD2wIhAPcSNoH4ETGhWygXA2lNLcn8p2mjk1v8wsImYidAvWUiKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyjIiNwdrStUvDXdtsq3AOv2Mf%2Fygd%2FASWAwQPdyqnr23UigoaFJc2s3IcyU2eLPF7fhgjXvAL8Nwbz7ncCI7vusGBfpEWzLX2sGh3u7FO0XJeyTqbxcQsknnsTth74D5v%2FdQxlKtwi4lQN7vMvcswZkUoSaV07SSBZNlazvXj5eMUWJ2e4L%2FZ3OPnfUeES6twZqVvzCql5xPrpOxxAzKaZuYkbK72H3r3X6gHftLH6q%2BaNmP00s8%2FEDV7Q2mxjdLkI2WbUJXQjVgHYQpWrjtxw5XqMb%2BNGWHfQg7LVFslHyqhNbeCYYUYdyGWneQ1jQXnbWYgbaIGfT9%2FENU6P93X4nDQhRUGrJ3v7npr4FnfcZMwwo9B2n%2BD9F%2F8wBSySa7x2Y2PyX%2B%2BXwBamY5wGyDooHibnGY8kFYl61PgsqNLPbqGOXH5Spm%2BrwmlQVr4JWf7lgGBukWSZBnt%2Fd9XRjMzICwVwSnT7JouoPbXhba9Wz0dgtTZoB7wweXhzPGETxWmkSC7uyAaX0aiu5lyrb%2FUVSbJxJaymrEbk5%2BKrFp6R%2F%2FUUUN6onVLSPNiyXJNtgxXdqU8%2FSMO9b7iwSNXRI1QlAb6TlLytzIUKrFrzU%2FaIIOKzcE%2F%2Bu8EgO35owSywLSDJBwioz%2B4r2rqwFjChuYTIBjqkAR06bd%2B%2F3N9PVbe0vNk9QzrZTkIB8e8oLpKfvN7xsrb6wAFRHjnv5UdTcyF%2BkHd3NysE5NS1jlhwqCUvC9AGvS4xLnmDAY2J4qJLOYNJjR2%2FICgS70lXmrjPBrTeJ%2BsNnK6ggsg%2BISEfVF8M1wVTowBlpbjnW2PguK9CF0W%2FP5mfWYlXmyAMCTF%2BYpQX8XIgLSeYKq45wxCKvTIEXRBE6HWMIY4u&X-Amz-Signature=77d53fa4ec62b6b800b530f03d67bdf11a6ce7f679785e4ebe1adc00bc6c3a63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

