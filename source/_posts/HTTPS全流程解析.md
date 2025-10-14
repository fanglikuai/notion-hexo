---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZCMYXXK%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDH%2FQe4GIth5PkV%2Fx9z1i9wcCFqRsFjp%2FYBOPPuaPDPvAiEA4We7GRgQqBB1CumNVOr2NZHuqSyRHdinGyviK%2Bs1PC8q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDGiDFEPKP3st61Wb0SrcAx37gbE%2FOYKsSxLlEZm6G47wyDHIcKMJvh6ehXMusTBpOhKXwYgnrQb%2B%2BbVZBGgZc%2BI%2FPuetwypoHrLqcdbSclJBJmfaeIpI%2FYBX3G185q33ty1DU%2BJnsp58wEKmAVEBDU1ubA5fX3Ju0KSknxMCp7Pvb6TbsM7rHZqSXpSSb1bOlOmYzCJgq8eeHrHnSgOo%2BNwp5BfGlt%2BDyE%2BlZB2TaO3U5Qc9lhNpki04j33wUBS0uK7vgsmKqIHG%2BIkyvNb7zblw0E0ug1ttDaKgXNomjua4ZjY2ejCiiqPt0oJ48aad6MjgnMDRCYD4WYH0VHQwi01Eq0Ha323VZQ43OGMxls2dNIig75l7%2B6JDa1bJbx8uVzco7310FEiZBxLX6gYS5z%2BO6%2BHcKiDXx3NYzJPA8LiHtzgfbeI75VsIhmREdwhtwIM1aFRiMMnQtXg7urH74aupdYRy7EheUPsmIMH83v6bfU%2BoZ26ii7GFU3k7aerTj0CZOMSvVY%2FlJMZz42QvCnKv5HD%2BOKJFRD1xcTGMkP8GiPPTodqnZC6U60f%2FaCCtLPdJhRvrz18zwROUTgVl%2B9l0uKIwLuh0LV0qI81S88UtE1Zl5Sg%2F1EZ4aZYcjcqiDB7HUm%2FSS13zGi%2FQMKLRuMcGOqUBAqndfdg7e0PpqrAJBvg7Lo%2FSmy5%2BEB7vZ7aDx%2B%2FMAJQ8RTxQws0oXwCZYZF7VcoxPpedDAFlqFopS0UXRw683%2BhOTXU0TirCnQ83xS7C1WBm1358%2BPArT9QnJ3gwDBnBzEzPNC9PK2tpam69djh8vQFrPd%2FAWHpMZqkfe1N7ct%2BMYTQeWGWQJxqXYRmOzZ0KeVKCf0I67mvdaktsC8Y6SLd91P0m&X-Amz-Signature=560860b017b877ee3f42ecafd4077e75dc924ace98b20c95795b38fb1a0de2d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

