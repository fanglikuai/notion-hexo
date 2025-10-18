---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBAQH72I%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdmZfCbi7yuOxHM9smp8tT6pICJ7D4vLJt%2BfFNWqT9dgIhALEP6bSWhWSWaRaM4YyqjU3oYkFU%2BdXONnW5vZkvwwScKogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaYQo3OGWfg973HEcq3AMxX4P12PpefoATQsfqfzVni7xjC2c9B3MZmUD%2BCtqPvCCrESw5w%2Bk95wikbL2hID0qkyWhUyFQi2q8FNhb%2F7CvjaiDLnrFGJAeculHIqB68Nrf6FxY4HxWR3VQ00Q3U5rwPahJQBH2OcvUK2ZiPAvX0wOkFc1GhiHCmdYWqigWtG0l%2B11BAh4J71uMImsrKP04LSQaso9WBE6POAmkBuPmeCPdijn9DxV98WKg1bkdHPVP2ya72VMgeyaSwTc1oGrFCSaaLb5T3j%2FpZR%2BQ9gUVmmrKOji2VQyBlS%2Bf%2F0DNV6WgHFZZaCrr8A2SKpmfi5DySbrmfy%2FSd%2FjTU3pZbwJ%2BZyeKwguyToJmtzkVa06izTGZJQdGvB13xfRYUnkO1amIQYbAIE%2BLfNavwscQtxSY0eVDpdnLSzwFnK4sB93QV0AhD4m0OhrqRkakAj7jsH5qJNKAkw4qWoQQSQ6FpQtpfqysYoovzyjeL3Og75xHFBkmtAvL3jVzN4goJLXAweSk2g0JJw%2BUEnkKRWq%2BUiL9A6AM%2Fujq0j3xK9Tkm0MXbCK%2FTntQL7PxFo8YQvYC5NS7F3%2BLZEPG%2Fk0m7yD6cm9WvrWmQNBbuM%2BRu4CLLYPDxRUAOtap2bWRkDEuHDD85MzHBjqkAV1djxi769IjT%2FA583bVWU2bVxlpGx%2FS7h0q5G0ogNM6WwrIvnSplym6J7vz%2Bmh6DyBm%2BZAAL6zasXpUoPSHGyFGVWXfapAtqDG2XAe5vwul1getybbDcLjePIfcZ%2Fp9YHR6uheo5qrvf22V8FUpTXWJzNkJvVL6%2FsIXD9T5%2FIRUYbr8us24GKh9a6R3acajMAlAaAeGN2t%2BcX99CQK9sTgcFqa0&X-Amz-Signature=d247980d24a5f6220e83a51df33f3c2aa582734aea9920575ba12f030982c62c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

