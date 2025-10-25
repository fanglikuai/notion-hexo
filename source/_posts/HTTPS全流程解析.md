---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WO2AS3C%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdpNCyZN0A7pQ1Ot7NX7Po2%2BLKJW9%2FE8TS3QCBa2%2BIOAiBy702cVrYmo4Ram4HpoAtdxHPW39fbNiLtaqHEVL4fgir%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMFLOjHE%2BVDbV0Uf9CKtwDL0E8wqLSryPSeUOuO73rV4yIlr3P%2BMAQsHudPBy6HnNSgalKccMxOJRQJaYfuhTmPeanVUI3e2GmijhTCpetRNL1EQA8cfH36P1I8vu17W6K5e5Jqqzska37xJ8W18bOBcxFLWE4LaI1eRZHcdQlWWr1uy6xABxXEp0xLg2tJWpN5QOsOT7xNQKw6mTFobTBU6IbBJo9NKAKDngzaRMDq9HWdg4O%2FabSyw%2BnNSOoyeTtVUJxMLsh%2BVa%2Fwz7l1BbkVIUo95xFtsoMnANCrwIOZcsI1%2BViTSjZnEqKWzApZWh5gxTmCCb%2FpOSjaI1D7FWyKLGJk2VLTFRVePiGipYOs5ioPkGUx0E%2BYe9WyT539cz%2BMx2EO2fw83huBwje1GU7KchJKnT4dW7rBtE9LU0weX3a7uew41d8shdGh4J3VfDj4au%2BnOTBm%2FJ1KJyhMeyWwcfBP3crkStigg%2FZV0ORwdFe7ebUlZuYHkkUvdpbwR7u25SvoG%2FeBYajRQ07iOIoXtXzNwQcfDrtmeOXRKJz8EEPw5JE6cz8wtJtND%2BIc5WfTCCfRVGFwx0sHzx3zOZzuV3FoyUoJnYvRKuz1F8day8HqnpcOPnXs2GZShCCd%2FE4Ls5dmH2dlORpRrQwtfbzxwY6pgHNbzbCXIFb%2Fg2S3%2BSMyUBy%2Fm03e1HT5JGoBztUetw6KlxpkmVQi%2FHz33XWhyUFqWMwEP2nVAMBdNnKO7Wt%2FaEMVc1Mlreu5UZSBQrOG8WZbCaPj5iqqbxSksnNmXpEBQmJEqK4ODeVnM7TNX6xz4ee0DJ9OOiJntciqJH3QO%2Fp574wpMf9QosVMDFvmVzwpA3IZWtIGxw%2FpAd4bU2wEXiafw%2FtdoR0&X-Amz-Signature=f96c4660835391b6c8a02999c24c4f1cebfbe6f033040aa81af57d983af518c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

