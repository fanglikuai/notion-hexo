---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X6XNV5K%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrWv6StsEKVow%2Fm2Xg9FaWxdGMO3uGHfnsHNnxqlB4tgIhALkmgw24ytIqn%2Fb1F0BqLsjAcUJHQ%2FWIAF2otJ38RppVKv8DCHQQABoMNjM3NDIzMTgzODA1Igzhu7xhwCklIC6rZqEq3AMyBOzuVzOFwgdTdYPXzQLgRY6%2BNpgmC7LPwuvp5R1fGA%2BX2DRkNh8BNAMMYOvZa33BsHA1rsnPeGZe7wEBzkrCwlTCW6r0Z6GyvVrl1IfxnR24Sb%2BuJY3Lv2MydflM6rPo2%2FQBu7EgKZAXVwaFeBMlofDRe6zr0x41IFyBoxztu4mkptx0td7ZdusvdqDmdtpIcfcUzl60unoAh2nkWGECWsXE7MqN9DrfhRKgRrtYbPcM%2B2w9RpUq1Epq1Y%2FmrY23vxyP9gGLjho8Ol%2F0kZxNWq%2B5t6sPJ06zANA3CvFC0QP80A%2FVMZ6Hy0a2nB5gDrbLBVyVZEJuZFbQQ%2F4evyHiNTjd1aTxs9pHYPLXdLdGKTqQh4u0f18I2qL%2B6oYV%2Fdbp8T8p%2FtriOg2luxGD6wIjW4ZDKXndt8P8DSURbqCyT%2BSfgPROz2Scbbxi2Ff1UZjhfnQsAqnmN5XH27swLaZtw%2BQqRdyOMZomV6rrgl%2B07vPQv%2BB%2FbRe4NiPKfOuxTEpLx6x4qP%2Bxg42Doit%2BhHLJZLutliEk3hehTttuS6UHsVrxU%2BmV5eRz4aXk0ikFVvkSMJA4%2FMQiBRsgGzWN%2Fg0Xwrgy8jsjikto3SUGs2hkRKVaPZpFiRPr6Wd7iTC4vdTGBjqkAcb4uOqQ%2BsGiS6YtMXm8MD%2B%2B2LR05NgZuUoxU1zJpiz7Qbq%2FjzRSjAu%2FqaQfwWhtVEj1K3O5PD%2FZ%2BaY8LkYmbPZN5t6k6gLMRcsjAndFukegNKpbfG1%2FzNC9Z4hRezCUziincjB3ZcvaEqzBZXuaW6q8baJC799YfWuQfefYs2exLfziEgt3FxUhTxAeS2d1iLWBho4sVNmb2JgbaviwV2sECw2R&X-Amz-Signature=e48c683d4fe76e70f9760e0dfe0ef90e8b7ee022bf900f8a9142a43cd8b6bf1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

