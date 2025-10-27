---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GU6KW5W%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFW1NVokSZdlK7ecr5F0%2Fp86mjxPgUN22JGf8LmIqh%2FaAiApem3RU57p6rSjRVgrxDfZdWrBCBA1eRiiYnHN%2B6%2FLsyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYqs3TRotHhvlf5jDKtwD8k2bwjjNNm6qY5GCOecw%2BvG%2B5ZXpJ2ROHgwAvS2SgE0li9sI0OYxXmYJJ7KXKRliKEKDoFojmFAJQTjP%2BUIf%2F%2FWnyFV8KCS6wym%2BT%2BYZYHgIGW01491ykK0frboRjUrh7LYWk3lwZy5WkB0ks0pyT%2BRRxcMdGpRiNNu4Tt1J%2FmYevOeD%2B92wmJqaxksAHcBbtSwIlhM3EbNnjh2iZ8iJL3ELDqZdUD8Z5kFZD%2BckqIZGgWT2%2FTf6VZc8HyCM0qFeLCUqK9wMkR397181%2F7VwPtLO5z5B53xHDWRDy%2B3BpHhcXIExfWRiv5vZcY3jcY9EoPLsleKsg86FjPc1fRm%2F557tFiBM%2FewCmeWFlt6BlD8Vm3NmFv9hG7O6wFE6emktSW6M3cldj%2BuXV%2B0tYCUhimkwEWuegkQActiqFvH%2B0uN8mlSIXwf1Jmcu4BwORzJAQbrevNIt%2BKNV42Y4z9cFuylYjf%2F8IZbbxX6Dmp4BNb7z9aL0A9%2F04UosgntFOJ4SKnqtU9LlR9SMPha%2FWxhEZqkSNMsO2uExnsvd2tXZkFpV7xZVVdJyZzZRMjlsiQUw%2FvA89VLobHrf%2FQmW5Zu6FD72POxX1aXDDT5Hg4bZdDrkRWHrtg8lT2qv1L8wprD8xwY6pgEgsFNpyLdE1%2BIsxMFa5iPpMs4JGBDjLCU4g9vSGeOMWP7sfWo0z3XeAPvHWQrDDNaO8MdIKj7lLUXHVjnypqzcPvYvLOGukBkb8AJrDs8uZsb%2BoTDGYplb%2Bco3LmVp9Ltw6n0Qq5nHUvjqDNr0AiwoD%2Bcf5o4Wf9BJ67wA6S3OjAM2cd6WbGDdz%2FR8K7YZ6Xk5pZs7PZg%2BrwXjWU1kcd2Vv%2BAvrzIW&X-Amz-Signature=909b3d71a8939da17c9eccef434538646a95a6a4ac49eafe24bd81ba6a9633f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

