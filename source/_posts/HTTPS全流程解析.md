---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QYYDIWM%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T110054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHNbAVATw7siWZll%2FuqpKHqHcKy4c%2BvCbE7ay%2BjG%2FsiQIhAIBk9HXtc6wfmNUKenHVDSh6XejY7BfeHViUdjqa3yGeKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxzcZJXOtSK4qmeTcgq3AOtYmnKWIvAQ4xufGhIFu6LlOD46fCVcumLNFtcjBLEOWDEtx4Z44UJxpsu3HNYLil%2BfNK5FUSGrohAyfcyU2VD4Bnb2he0IYlmV7Jt1reoySZ7e0k%2F7K3zO5458yA%2BoEQxgQZ68FxV34l420ff9EkIwhpIFz13YZv6jzC0LKEUnZVdbFE%2FxmUAnvnxCK2xvRhnJ5GjqzRqK%2FatCERDersxKxI7nCtYykOU74I56ElALU7Q2vpWmkQx%2FmFd%2F9559UValqIl92HFQYPmiL5VYv%2BsEKqBlZK24Rh%2BUyKe54ryOegwHNSQICW31Uz19x33cSlQr0fx%2B5c0ohXxk0sKsRO83pLM9rppBcmjezJTWUJ7CmNkPPO09lcOPSgqMEZ2vOTnpdtX8e67aFA4XVYAqfTGH9b6R29V%2BeFkzujsMnVnXKvpVuWF8Ic1oti0ZQxAeAWM3J%2BkIBrmSMepDNgzFdV9mgkIyEe7dWPVKwflvIiHM55pa62SGtFsJrdmnh3otk33gnNPzzO%2FQl%2B%2FERCt%2FD2J7MD36Nq8P3Dn9geR4Tsre%2FbtIbSW3JL9jH3utEUnfjpVNJoY6vGyYgc%2Bzpcfc%2FcbkanAFIPlyrdcmUT%2FK377HAhNvbLFxqAU4GfeWzCe4KXJBjqkAamjKqwnUHa5i3%2B2G2DVmtDU%2F67sWbm%2B6yKlMMCloDQ9UVh5a9xYUm8HnvLl5UoqPNPmnf%2FS5wOGBfRgaSNTw%2BN%2FzB6eueK%2BSb%2B0ueZ8LLveChGHxoork%2Fg274Vv3vKDgp4xfRFI1gLtnqbmi1wsqKSJ6duiSYRH7vCI5Mlq5DxmT6bcFZshSQApY3YNCC7c73d9lbg2ZYLFYr1tYM6WfQ9BFYrj&X-Amz-Signature=b395bf6851f2c3ac619bd1cf0f306a297ab684d8d473154ed66022ddd8ebeec5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

