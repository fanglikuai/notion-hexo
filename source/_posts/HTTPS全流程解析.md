---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HJA4Y6P%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZ%2FQ5Xcmbcrl6fqLHZRtLvl2hrKS9BrycZZi%2Fy4wa1YQIge4RpUUC1B%2F2d783YHUnA3H%2Buf%2BOa8dwCh075IcU7ivwq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDLz0elLwoY%2B1eiWVQSrcA25okgTVBwN%2BMJP250x5So44PobVGxEc1475hwvFtB6ZPGbCSLLgAuCzu27j1hytsrB%2B%2BynPxQBaJW2QigYpiFb5ZFDbblArdOdy8rMBOHOHtZA1T3RuTU2kYXTfscTpcMiQgUdi0%2Bupg5VDx%2FZULlV3KYcQpBDrGBoKW6nnBIm734tTWEkCtnHmgAWOtqav9BLYfGvGXstz%2B32yyP%2F87IgDXVfGAUQqZQTCQgb47YezW4TT8DzMZU%2Bw9k2MNac06gq2a8xTimtOXaiNX9%2B4hWw3C7HY506ZHsgomjSm2M22NaC2pjb8S0GllHfxrO5OvI%2FQQXWJCXNS%2BF5XuY5fWqj3qYbju84KJIZucHtFtVtvAY%2BR%2BqDbcnMi760VwPIUZPzCbswmDd%2FXJctTAFX4B%2BtfJoWjzrxuyqlWZdMeeevngeumszEATP4LAel9FFAPUWl1LLNKAzvJwDrp0nPVSo4TvsNlFXtQ3Tjn3j1zR9kAABJ0ebn3w3JLrFtnSFOSEZVFU4P0A396U5%2BxaMSYonL6gvSc5k08RnNfVmsazaUGVlhB%2FsPCxnuNdH6AQWuc7pgYif9ahB5M3sGJSQQu9uKrzkos0JE7Ll2IBK5gEFpau0Qzv%2FSq86flWuZrMPret8cGOqUBRJzO1tX2kdfXp2KMVZ%2F6t3IWnaTclM%2BUMNYH5oS3xhTen3MT6mP0fu0IDMmPMSImmoHQkh%2BzYngoqSOQO48yL2SJTC%2F9HyAuRcDPVuimv7u5bZHbbRY0B8lG%2FIakSgBlmnIi1voawyWEoAISpkLL8eQLk%2F8D7Ql5YzLxoG7AL9d%2BGUl13SJqyA7OB5z5yJ%2BW9E05A36sWkZ7q8grZdKgLWjhC5cP&X-Amz-Signature=7ee158072862b2052d97802af767bdf25dc8296c7d24ee1a720ce5cb9a355524&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

