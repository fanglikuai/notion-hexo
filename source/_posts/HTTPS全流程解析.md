---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q6OTXMAJ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICadG%2F608L3KbG18FDEkYam4Mo8k%2Fq8Hbrj1btAj1CCvAiAoU51IlpNaN00y5lZ%2BLDFxif5iUn30Hs8bS7xAwrU1RCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPZ9kV0GCKVAhyOKCKtwD4v9dFCcwS%2FbDVRyQ%2B%2FJKlhl7T3em9khMACSk8gYRvTqpJAXJC245SnFgrXYB%2B8gmIACkyJ6hnOVvuOmoT7jjdMXkTh5dJIO87rC1vPtKpmSqd3tk%2BDjyB6x4FULARvsDGPV5CagRnx4Vsu4qwTqIzscYl08Xd9mm7BtpFIqDiCiMUg%2Ft%2Fcgs52GRe%2FOfEZwqQ2HVGez4SSb7z8Z1jYhJpFZWKV%2BsK8ZbMP0hjYG42Shj3zh5cp%2F5fBDwA0qRK%2FVKfonXccUerG%2BbU48x6ihedGyykEyw4Do010gO0zOUR8u9a%2FlzEhtvoAz%2B2QgttLIG1G3NsBCBpJqwC0ZZ9qSA57UWJbvZBJD1H1lqj9yB4um8um%2B5PHES0inxnpGn%2F3pTcQOuobLq0GzzLRZ3%2FfLUlUz01rXjy9%2F6Wx9ItDojI%2Fm9XwiEckwaHiMwLDYdlkUyIMS43kbrXm5hsEcBiWmzCFgqG%2B%2ByvIwOCMHlt5nnitPrEOy87c%2F9B67QJF5ajMBUXYJsIbRQA7M9%2FaB2MDtQnGOE6sYnFQ35evZtBMOl91V5ptEL9gf50Izo3HAxPmp7RhmpDX9vqNElAfATuMGiX%2FOfsOgT1mRWuqzzQT6rmcsVG3yMpYdxbrPyyF0w58XtyAY6pgF2beD5eI3gOeL10N8ALzC7evqaCi4eNfxiKyjqz6qyELbBCxbnfrUPBK45qKRmees5kj33L%2F2ZSZ4euQSRR86ONANQNtzibm%2BBzRodDShN5SJYu%2FZoBTIkbVmtz2%2BY4BOICBW2q0KCYEud4uPXxN8LS3Jv2e2S3hWBtO8jROorevseP9rtNXu9kl7uSgQ0naJnkR0%2FM5IavYIXBODK9QLj%2FRPOh3TY&X-Amz-Signature=557e0a23a79e7b3ac4c0495b53f9ae99057570999ef0eb05dddcd70c718fa590&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

