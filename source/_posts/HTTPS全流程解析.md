---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OR3FOPW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQDqZ4DFg49ql32yBXfMZBLb2ERkQdppgaTfQGVhSj%2FuwAIgNjeJWmrPAFJrOEIJ%2F675wXwO%2BBtE7rggaaHoqH1ajogq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDJ7E%2FaBC4h%2FB%2BRXIOyrcA378avRf5akymzWryEk31vj3kvbc8dCc%2BjS51qRN2TCvUDICaGFoc94Da9zFA%2BKe0jafvI%2FLsUZmGfC9tUGOHovxScKzejIXR9%2BLPQhRJ%2B1CHaZlXzF5HwV5EzZ1G01p%2FbSo4ezh7bOfNkoZES3uKZZ594T3wK%2Bi%2F36IMFsvHKZWWucogH0mJLUQkwhiGPjj%2BIdA4WrerfasxGMyYRHcbuDStITkfD8MmD19tlMO8IwF1SgsLQ%2F6If6gwnwquBFXXFkPmx1flOH62Bc85YczmtN2vXPZRXzv%2BISao3RH%2BoIC9vE9MtffGxmKp%2BsSd%2FG64Qb3bCES31ZrDPyKb4pfr6D59Jyw%2FYlcr5%2BU4m5xWljNrfOEDwgm1k%2BKUP9MHNJeC%2FhNoLEF68hEVBoptHg0rCI3ym%2FWZcw%2FUAiMgOm9N1lWidkY9M2VWl9au%2FpI0q0O1jdMV9GPktSRMFhmecqO%2BLcUg9I8DtR8oMsulfYfpydnozYu26YI2BvbH7bds6COO0r10hYh%2F5aAAxmCvvX%2FTmKZS94ZhflwmnbwzlDSgrwhvp7SqogsD2nPXiJUoN7NUozmfvVt45IeXzbshJ7OWB0ljrNc04AFrcxMcpe8VVFMCfA%2FnBvBYBJcBH9iMITpzsgGOqUBx4%2BFGIk4%2FtKj7RVEMHHjJtNGDokjIVFmKM2M%2FFe38Df80XyVlasP8v7f1P%2BMEOAEzZLY47PTh7CqD1kxcCVCfCEWFf%2Br%2B7oh3%2BAlQ6Y6wRATZSOiR%2FarIkuZ%2BrwERwKfZiKp7%2Fq9VvzVt%2BVnE97fHyPA7XbcKu4doVWtBCJLH3Jc1hYC6t%2Br9Joc3%2FsZCX1BTIp%2B0m1EJ9x5g%2FheKG0YLOZRvAJ0&X-Amz-Signature=18861b1e4095d1fb396687e61ac4e7b56e0ea634de30727110161ea882a4be31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

