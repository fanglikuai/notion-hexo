---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAQDN4BD%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T050105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBRQ7kPsdf3d5VTWYgSSMVDi429pHrRgkHGIrOnLZ2JPAiB60qM1Veo1ejKRCPVW115RkMnxzo5smjTStfdJ%2BK6WNiqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYy7b%2FfuRf00hkvDvKtwDuGP2Y0Jbo37Uky0pAcbPdGbFd6oGcegMben0bdp2lT%2F3njKnEUE5q54CHBs7LDXuS3od1tJ2QHkJwrmpyaV%2FkgwIX1uKxbVpoarVCOOhHA2XhG7GH7OmeCfVV%2FYhh2LPjueFtY6pv%2B0wmzOPRlPC6rqGKJtigc5gNpcbLJ6Xb5rII9EP2%2BtH7hqm1LUcKG1%2FJm1uA6Tb%2B6WgfHWXZAxRPq5r7i7hWBsHn1j0nOammitnl0H8Pxe9%2B3hi5gsW82d7JFQjy%2B%2ByGNeUMhuzmIoxwl%2FZgvaQwScwGZQoDpEOie3nv6R9l%2BeVN9TwIJRQGeNSgRGzk%2FpidvHZqwlSNPeVT8ro1m0m1%2Fh62DVl5HGkbUoned43v0KiZGc1qmon9s9bo%2FDwY8Tr6V%2FvDpj3EWycBBJB5Vk0amZpwnJAheZV3bnupZ4S4NpRwyC%2B7I34LOmirN0IJTzLM9JG5SPWZ%2BoAuNy98CPoo2%2F%2B1S3L74JtG7z0oY7xBqSAc6gQ0iRz3%2BbIJEA4M6qza6aEHxDy8VSl%2Bds7PamRCvpy6fOHW25Ex9AJNy%2BXMcq7%2BmDW4V7hjDG9xzztlrU8iNEd1%2BsqfrYDU2Wx1z1K1bou3vHIfYD%2B3eguJ0Ju0ZPFL%2BktZoww2rieyQY6pgGnJ%2FEwUFBRUsla1IQ%2BHH%2BRwSCNC4HHc2rK4%2FHhkUS%2ByAMyJY9z8%2BDPT1hXp4xWeroeGPzD%2BeTStSEbqUmf3JXaKPFrKrei1gH3vCI6y5ITOe4gP2kvHlYK8g9cnF8R4CT0Yp5P%2BrTS0IMBe%2FtvJFSKpj7%2BrPe%2F9Yz8V4AWYzgdKM2iV8MzrfQwe%2FqSu3aGrqHbf%2FeDQtX%2Bu92dEnFWAzNg1lNFtJMp&X-Amz-Signature=379a417838bbe90e90d4181bcb19ca371d7aa097192dbc30e06192813b1a73eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

