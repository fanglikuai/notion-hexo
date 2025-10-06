---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNNB24L6%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh4iLjdZsgRMm4fBB0YgIh4SWTjZhQlOFjr3N43FC7EgIhANlwG6%2Bs8AwJ%2FsmGy9gy2M6c4fVpxBa%2F%2Fust7conLMvGKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOMFy2wJeaHmLZ4E4q3AOGRkcS2k5FZi41ygPHffSQOTP0c7t%2F9jXzFVfMYkN4evFVjOL1V%2BK%2BJK7LM14rGDntamdhJcQMgeTVsjZ2bS7UK23O7GdsRoP6O2iMBfq8ZaRf0Y5WfSGjBB5Vuf%2BhUwa8%2F6oZM5pE3FCP0eeOpPv6VC7u3IotP%2BBCw1uacBR2x5ZuaiT330FcC33PmK6IPorT3c6PZaJhrSSIqCjFud%2B8pBLxYXvdvSkZlRbPHZ9hsmm%2BWj%2F9SSS3DgaSiwPtbqhl6D5gI6Mz6h1Fh%2F2jIWl0e2VJswcibq5CzKTuoI0iVmiPKajpUODn3bsA2wpympD%2Bq67kQPtmOkUjtEczh4Lr7UIBeCq%2FA1X5lEiKYSYC22dRqHIAlD4nYWPmwteKSRqp6j4teKcm%2FgHSydxwV%2B6MynMduScKViURZxAVWAQmR70MU8xwMTiTSW7d9YM9UPTly7xeOPO3xiAX7ldn9KBCpHFzWei7c9APiNEmZG602z%2Bzwbw%2FYsMi1TeJt5AdgxAHMWvTT%2F4xvedlSxx1VeMq2D6zMHoO6GFmt01sVJr4YJgk7Ep8DhoGYcacO9EXYwv0mS6eCfFGzoIWbyP%2Bne0LSwA3HzXD0vjyOACr9WElUYpquJrpu2xN9UMusTDuq5DHBjqkAWJOH37pMELjJnANrOdtPvDvqmuh%2BbbSxFUVY8GTWY0lCR7vKB0q4szzHOm5JChmFwRloJ2mL6gjeH9UJlZW6eMsDi7iRt3WQf7WtvL%2FqvWizlpXZHm%2B4Bs6pLyWmb3JCW6QKRW9GYXxKxlZXaEMp%2BT1vzQzGzL4MvX3Lif071c5SZA6SFprGEyq0Qs8DyvMneIg03lrkzS0eTObdeet4i40v6ab&X-Amz-Signature=04e241502def5002996338ee10c9445da577af68074a4178bb057fb4a341ef46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

