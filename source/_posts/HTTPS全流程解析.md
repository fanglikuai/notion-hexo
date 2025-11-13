---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PIL4PDA%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIGqsv7UxopkPJdHPUfG3PWJL6SW4m5meTAL3CEPm1CwgAiEAprzKv0jnhVyeMlRvX7N%2FslEzoigI%2Bivlr9wn8WvJ7Icq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDCmBp9sZa7GpA%2FpIZSrcAzdipK2kW06doZRY%2FHTJhk%2BTPSIZKJoAQrt0j6ww3utHGlBa5jGMoO%2BN%2F8OQuYh63kYGk8Bvy7JaL0oEkza%2FJAfBtbxh986yF0MgotNGOqvQmSBoxunvIRq8pFQCm7xrMzjhC5wVJN311p5KZWm8jQ0ULZ%2BlmomzT0iFnuD8I6z6VgNhUjaxL5EjajJW%2F9F2NarSQyMzVCcJNykbfh%2BHj46wAGspJYfUFrEslml%2F9XdoFBfGQWt5jzUfC0uYIIieVm4UnNgxzWXtgTiXD21fMIiTJuwhEhzBBlga50sKnGh0qwwXCIQiPcSYCtMmEoEHCGPjVWdPU%2FykZT0oVLz1MonjfDRaZhaa0XRX%2FOGuHIFOZzaYPuW23QXCJU6zy8eBO2FVTyHGEYvvx5%2Ft%2Bw6TCm3DcM%2FJW7wlBQ4u3H%2FLxCZz%2FSOYl0rxBkuCLEvz%2FpRY9FCFKj1M15a89zwXOzZaVnsA9cWjoHKtfWhDRIHzH%2FxJhujB4hGv8XDqnY%2BRkYaX5xm5ZxLx8%2BZzqCDOcF03OB6Inr%2F8hKD5ChjzS0by3oPugsrsfO54RkF0rqiM%2BLQqomwDbjAdVujznH6r%2BuoFKxHsGQJvn0LevXMg5OrJ2fujpw3xgZvZBg83H3eDMNjx1cgGOqUB7jmse8s3G1tIS%2BNLF0UHgcfv2EDr2b7YsDeZdZQiUh%2FJe6QIYyhO6WyaBbYz2AAi0BA2lchRuwbQp70d5354WOLPMsadCN%2FDnV%2FV4GS%2F1Lm%2Bfo6CNGTDH3qgW3oJwRuxASln5kmg3i0z2JXgHawkN86lj0ESJeP6auKJ%2Fc8RelCb%2Bu0emAklf3XMV88CO7cxIlL3Fc8cG4zpn91ed79fEu%2BhQEiV&X-Amz-Signature=0738476c569bef80be75585fb6409966fcdef15523520e878e8bd611130d9c2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

