---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQG67ABM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBe9do6qoyKN7i7PervhuY47T3oMkNfFERBbUgM0euOtAiAT6PcOu1Kup%2BhYFIh3C%2Bv6xRCjoH%2FC05R71uya6taAbSqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMA3e0On%2BsRWQaTlrpKtwDic1qMa1ri8zUU2NbqiepKtKgyojFFDFxoH0DyH6Yb0JusokdH6%2BGy%2BSqkfxsMpLvCt5OVVB7luUFhHt6WAIHCwuD3a79OvYIpXJEqC69LgiSWLzi4Y0uCtlEwMEUYGWzonZ478qP6QEqyUtlVsL%2FMnLnCeFYepx1Ap%2FNnYNgzRe6%2B2breyyfqSTXOWv%2FPX%2FX6lFU9RJeuyYtMcpaQUqTGgjVqY3y27IjurHD6AviUwohE18O4xwxwtrEg%2BIvWEbOC0P2ANgqGJwS54nV4vNkee9WHcxi5%2BOOlaemTuiCvLBdoRytTDSNHzQl0omr0dWDt8bpX4F7gwrFDmgKZMFvNJYchB3Cw5rpaxDI7NBTvNQYI3ih92YsLLFAbImnaaVCRoqVI2hQix2xfzp%2BUzPZ8S40ugqeztfm6YffoPH%2F9LHW6T%2F%2F07Ef%2FHXJzrU%2B%2FbZOlxKYkcYNT3jKPhVJ3bNSrvtcY6ttJuDILtgh0mXH4FiMnf3C4qsonRc2WiJ8b6OpPxoXsAvQ1Dup8C%2FxKM5wgD6n%2Bws2zsy7XB0bSdHT3MaGkj0fjF1kLCx3RZE6wztO4JJL4kFaKW9B8euXixd%2FbC2%2FTobUwIxrwXjUYQ3N3miwVsHnrhdoueJ1KK8w4JTXxgY6pgGZq%2FAT8TGTbNI1bzFMuRw4wOQxpyBhO7Px%2BowX%2Fsyv6X8Tfd8Z3kZlxyd26vvELKhAiTUShyNOneno7cNv6MD7V1hJ3I1YlYwnozJoIpiPm4gZMnsZfpPMan8tH768onvPNWyLUr%2BMa9Vp5ip%2BX%2B0lCDcUrldIPAfIC2ebFGWzmi6fwekZ6VdHvDaY%2Bn1%2BYvZfUsJ4bcZyiYJLZ8rGe748cmxMOwCM&X-Amz-Signature=b813694714dd4b31c529e021df83d2f142dbe3b0a7e9e9b353fb0cbf87a6b872&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

