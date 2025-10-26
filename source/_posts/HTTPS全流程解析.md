---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674ZZ6J2W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICjfWpfZ45bNaqu%2FMVQwT%2Bb3GH%2BsRzVsaEW94qRVtI5mAiAs3Gc7L5EwxEbR0%2F9N%2BWsf5GKPujHwyVJegIhiqcUo6SqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOsIG0A%2F%2FxO0l8L5zKtwDrZ0AwYLQViaNlIPs2%2Fs9KOwzn8y1f2Xm9cZ5JWuB7KKR1XowcE8iJVS0uBsDiYzzq%2Btx3bPq2dMKbXW%2FR3Ch4HtrGlhij7SKOp%2F4BzcYrmdgAYAxSummWmDT77tkGtx%2BpMnsMy6H9H3ATkjmtah3h%2FF61RhvGr%2FBQEX6YMDiKsmG9cZPz0o7NDZLYG7W2wq%2F%2B9Rgd5nxroDlofPVCxn7KWTi%2B3zNjoLJykEm2ElobIyCRtKo0Ezt2gpTPXPR8L1C8R50%2BOKpK0hX0pSuE4ZOORzjugNniBgIFpKlikZaKVRRhCxRe65CG4buwz7w%2BoEpIdS9Z%2FO0IXPbZ1HHInphdrZeNta%2F0MIA7ZY45rC11fn5%2B3YvqphY04Pg%2Fkygk5sCtpeqtat8KOb1uKWPJ2Oy5uDtNF4zlU3d%2B0V2jVHTGVBWJWvrVFLhMXGhhynxumTdS0%2BcsXLmB6ejcN%2B6WHegCA8Sl2%2BcCnyv0CI1FEHn8by2DXFkS8MSyl2PrULXUFDe%2B%2B4%2F6greg%2FzTjlY4tEs0g8Wzp1848KnmT91EdZ9XdjXMZ1%2Fo1Fypxr1U6eupX5vvJVzhRR4zudVr02BeSB4daeE9H5vD8hjPxAlVb1bbj7G4h9PyISZraB%2BiHUowhYD3xwY6pgHzLeuUvb1eate741xfd0KNMd7vvvvdDU%2FsGd%2BVjTijurh0jesG6Ba9rqbHT80F18PB0yg8a1RMGm4vgtzk0xte0ezMq9rT1LxplMf2pZ3wsMePb7Ga8%2B6QIh1TNa51nMz9oL3xWt8pUoXlR0kGuFovjUiaX3Sxf%2BGw7vvTNnEjsMKHqgRqC%2Fm9ZHuy3hShi%2B8pK8DlAUvNqQnZNQsILb9spFL%2F9ozY&X-Amz-Signature=602bef729ea3bde3fdcb0e88f86cbe63ae1822878fa847d4a8ec118f637e2e8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

