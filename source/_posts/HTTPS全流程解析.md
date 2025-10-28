---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NQ7MCK7%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T110036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQCUUBJozGnm1fd2bdikHWDEWGT5Q%2B59eRNjLmtvZgJcMwIgXimAnVdRHrx2HwE2s%2BOGMmjlnyMrCOvm8L3W1ZglA9MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIOVysaWBZ0DUFQ8kSrcA1upcuR3vTKfe444KfVbh4z8ZZc15wHuSHpI7pwm60dtsmL9VRLbSNpRFq3hqj9wTKRDtFPCQPh%2F%2FHKH4Aw5N%2FoYvbFuGknCRcVXJ5g898Q3rbdlQu9LMU5EG8GDXKcduh3%2FQ%2FyfuBmFJYOvWetV9tHrIaCU21PEU8Mc3StBEgWuv6uKnWziUHEr33MpM6ovkpvfD2zb80ypzsYzL1JuJcWMRVchprO6aum5hjkuWu8gsP7nPkSW7v26fZoJKCgr4Rx4b5I4mtOM8R1A84wouRNLZJSGvlPr2Ss42mcAjrxr6TiBZP7FhiUtIFxEqk%2FYp0YlI6wCYEY6z2HNKB6jr5LBAjiZjo60VR3xMTfj8V7zjyl40i64dqGIVhuslvX9u2yTjnGaboj3TOfmO%2BdmIJUZmGaG4lck2LAO%2BrFQeFbWgNETZyrPLQvLuDa4LDGDRd1O%2FjscYBQyN0j8xqtVRZU9YRmMy9GbXGAllrV7uwumgCABbpdhHiehEKnhOW9uhY%2B%2F0%2BVwknFbW2q9ZA47JLAvxe7GIurkECtJTlmKCmVKoaCli930wJ0c0H8K4JWDM0xFpmG1Aq2BTicloENyi6X0xIlI1mt%2B5Da5d0725MuWUdOamvOv7unXkvrWMPOmgsgGOqUBoq%2FSrto9hWp6VXRPbm7QWLO1GxeMaae8eMWGCANBRMmahgg%2Fane5kWdD%2BNYgt4kqeunery6jrhFSs%2B295Zigd6a3b3WtycnLKXG3IP%2FpCqYyUIRf41%2Fen10Yx8wHyZMbO25fz4KXVx%2FvLW8M0pGukzCCbs0Pca4cxLWGu8WT5N98G%2BLcgho0EvxOyVVkFjA7O3kESFp1A9f41MAlnRAVqGmXXhid&X-Amz-Signature=1895e35e4a8c681df063abfaf622e2d75e5b941b4f1d5792d140f81addd194df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

