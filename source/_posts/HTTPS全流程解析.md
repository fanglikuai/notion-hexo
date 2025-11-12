---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672OJIPD%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQC%2FK%2By3%2Bo%2B%2BriMcDR5GKTKidA1AHt7jgccz%2BKOusAmHIQIgaDjt9sWev25KDjuUUzr%2FHc0lNV6TnDe1PWzJATc3aPAq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDJvsSMGPvu4GpHsFlircA2BRu8ix%2B78ggbOZhZbVnNGsIrjv2TH%2FFJ%2B7XlkpAV3fJfVNmBizhuWaLok6COPlZxk%2Bi5B0CjMqMVKFc4IK7fzqBghcOI7MKFEj%2Bf9n2HA6H3jPjgpmxa2v3uioMs9Wsfg375fIAC2EwFhabjP9BZsjsitXu%2FuR%2BuCOkQzE4YqU6UzbGi5x2dTSnoBKZImhYm%2FhhYNUn%2BysTRlpPC92o8zkqtCBuK5%2F%2BF7BONG4XFFK7GFhArY2%2Fb0lHViMKSFfeUoed4NWEBI2MiBT%2B333jHrB5F8rmY7gbMbca8AUpv7r%2FJJaJZuDDXENbi3myeA8UAB8OO3FZ5KxiakSTJOw%2BGeoRQ93pRKhKXt7h5rJr4Ob4vLHdgZ7KU15d4k7lMNgQDfSD6vGpmT7x%2FL3%2Byh3mevCVjOFjGpN2y%2F2RmYxDPTeFsj1XSjXzDPDdBbjO8xFmKxkFsJZqge6YAc%2FfPZOJC2mP8aY%2FojWO2P4id8Dob18D0p1vLus10EBF2npWbopCFI%2BD7nIxef3VXnq1iYvSnqIQp5P%2BPKxuh%2Fzn1XcOR4O6LZ%2FpSdMQ1rlGikynDEYkz3IgM2b7bHhDKLCP%2BBRYSx9Ql8Wppb39etFfCNwRFegvUI2AQ4Fe65fvnUaMKub08gGOqUBcJ5XpxDyvSGE%2BgXNNh2U4EJo42WhfEym3OJD20IdxjCLLEOUEvaZnBV2lrdBsBfIDmYyR9y9rNsqKtdA3IySH82ytOfnHWqCus%2Fy%2F708JTrtirIJuGSp1p92VHEP7p1SyrnHIudOFlzsqxo4hihtMywcFLo8%2FB0e80BroAbYAhxhQy2n0hW%2BHFJt72uytheFEInauX1Q5YpbFM%2FxGA4y4qu1bGz3&X-Amz-Signature=6cf51620eb622ccd6da0353ca463ca4e0f957a7c5ab2b53492fcf2d386ed3450&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

