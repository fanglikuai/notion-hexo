---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4EO6D6B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCIFSDKruO%2BT1jy%2BGp16ceEUnBvhF4g9ilM7fnXplG6%2FzWAiApHus5ocSAhGa5B6kOGb3w5IJ3eekWlj%2FFKjsbX29L%2Fir%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMkCKFYhlvz%2BIiKQenKtwDx9hN6kecMedU2KoP4gFrXy%2Fa57j3w8QvkTYc6UJEO5%2Fp09PSLhQoNdMmahJ9cOv5N0TEWmmIl49Wd9%2B7MO1XTLW4N8CsYvW7msH71wpIK6I9jzm%2FJKYLnABVQ5kIDQVA8p%2BQqQFjLnZhZiuRD3Ke9EqhxHQE9SFgjuu4R3j2KsoInJUOBE3kBkq2ETqjAxo%2F%2FYhWQZeWBjA4Plk3gdszEGiKS2nFCmxCbj8MW5AELB2bA6bbphUloHJWsc%2FfIYYlYPt01OpnYdqj9eNKyWfZsKS%2F4a6eGLxlH8g9C22z8s93g8kaRvnzgqovLhT2bwkt1eQ%2FtYoTKBW9OE2PKx5t%2FfIgKqEAWNr2TSgsmLbA1j9Rrl1LRqpurhFQ4gjHMA82%2BWJVC6Wg6J8CU1QJaoFtGLXBzyNigT4MYU98qE%2Bb3bhywCMl3D0EOv0LQ74%2BlSP55qL4eQQTk4RbB7hr5HcE8cNonZcrjxnyrzkkrhsPcBCGLnGLAAl8YDK12bxoCoZ1qqHPKeHJG5S%2BrDOso1n5xc0EYaDynp3%2BNN5zdHi77yCKQgrPPCmiJrV6hYfrjvUKXucP%2Fie%2FAWjKSCnPG%2F7lRT%2BCBzK7O2tzcjYHVQhdsMh8cq6OYv8i6KwJIxUw%2FPnPyAY6pgGILR61R6D4cuKnvUTK1T2%2FCjvnd%2BSJ8xwl2rBmaosowaJBW9IKmO2Fd25kyYCRMiE%2BZ4y%2FoRr8aPxI%2FhChR2%2Bor%2FzGYX09fKENKrQlsXEoikXT9VlpW20qghP9t35QkpPHa3aRswLjObHeop65s2zb2doxUCgd9A96hblUCPSTFmzMzvcBUrK3L7G3UFmbThzKxWi7dCHFMhnf17mQmamZn74Hyhc8&X-Amz-Signature=7babd4994555f07ea6a0b1e1aa58e6f0867b121812b09230ebd302a0f8f7063c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

