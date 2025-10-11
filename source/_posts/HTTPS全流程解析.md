---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULS3Z7P4%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQDn4tRr1Uosmg7TV2Csio76OeJ9R0jNd1AVy0N830UNNgIgTui7dyX33rFgjhUZsKawEmbXr9b7bU3GtFSnp3UVTvsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDC1%2BoJpwgeGw5EKEFSrcA8KmvUZJBbM4qp%2Byp7hjNpm5%2BVHVxCA2Ynj59PH42keRaqTbzbuVXf%2Bekf0J24fj%2FK7SszbG39zoItXbMUH%2BsOS1x1s1WRc3LHyQAjI9og1wRYJbLMr5r7B7Ut6cGKfZEYXL9rfhNSyH%2FAynrqf5tcdYeZQ4kaZf9kO4%2F0p7qlFeBkCuOoAfch8YScO8VsDT%2BSYc5QBNuFtwZij9goJHZL6NHXcwIovIrAOLa%2Bxp8GUOOToAWQ2o7G3H8OMNBZNoDy0L4ve0IigxsUUMtFCA7%2B0iUKE%2B84kgvRP7pYzvOE3YOEzHYPpLyN449hpt0qNlss%2Ba2DdDMXLVJpj1iyohLzHzEp9MZ2BWlhZKdB2pKzyB4gR1jZezYxjpldpyGUOFJaOfrEdJyP49Oq8EieHkdPMR6P57lCTC57F6ztMmZ%2FC3hQDYqMXLnLDJn3jDBulG9ye3cgF991c9NAKkjQCw5VQ6YzMs5RfFcmPXDaApe7XhkJS3wtlyfr2bki%2B3yiWVivWd8PIPcMQUOf4MKixxy0CcUHp%2FAI5PuRWI43eYbhlzEEo3XK%2FNIC8s2yYGqgXg91fUgD9HfyzEjPAxmq8egObd%2BnGoePptj80HfXmR2UjzoAGpdAaZxPnCNvOkMMrEqscGOqUBn2d0Du%2FynyL8FliaXJnA4nor5TZ2BlNuwE2mef1v4uOXIl7S4sFMptazOnKY8eeyR5KoDZ36sbR8zBMn2PqKlGoClSIk8t4DvYGiIwdNSyUfkcVtupl2ydNmSjjm6auu%2FcR%2FGaF5T4Ug3nVjgpU1kOznGf0mO47mSh0Eg0pgip9aW%2BZCVzn3UZI10V24yha7lKIOeMPWAlokRcyAYoXBKD3SEIms&X-Amz-Signature=1d6c981a25ee2ea2b92ad771518caf1fb7092fd417bbdc130a6c0b1e983d0e23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

