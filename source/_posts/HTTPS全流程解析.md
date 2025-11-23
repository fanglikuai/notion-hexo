---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUNMV3G%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIFevjg%2F7M1i3Q5niqZS6NZSaAkRHs3qYJvtjPWSGXJRLAiEAklhFipRIFIrMF6uqGEoNjMSkub6mLaPk%2FWx3kcinWv0q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDpzPmDdXWGCRGEeXCrcAyRIejwlwUhQoANlEs21AV1Whqlw%2FgE%2B7SW8n1r9LysE6uK%2FJ4KZg1WFC%2BV%2F27MOo8yPO45VLaotoa%2FiHql37edL6xJzVxR5Ok0xGOH2JJk2Dxwn3b6EMZx2lA271kscaghQs%2B4TcpTorlLhudUwSHHFAAqxmKKB823YHJ7RuLn2jPWkkybuZF4sRq74uzlKrMqP6GNTJXTLD9vfKA%2FZK4UgC%2FXc0nBU2ZTGU9preiDNekMsuaWOYY2VgPlRwHd841qMl3MQ9FymXPhNiQLdtBOyl5RNuO9PloxHQsnCWJpxdO%2FKh3x%2BvHgFYIXwdQB4bO0KJ6VI%2FMa9IRr%2Flwlh5afBgqyd6uo%2FSeOr5BcDMRXeGgqIII5lBWcZdZrh%2BkTEU0bEv%2FgYlJH1k%2BkMr1hjhXvnsAKsl47%2F5ybAlJflWt5AYiE2lVCdGxwOBjSNs8ZTuijM0QAKKi47jmQ3Xw9BxETaDv2BKTHzoq11yE7Nc%2B7FwWAtxyynP%2FBBc%2Fp4DCqirHygASTDLc2HiIosmZUsajfAr3zvTViDZHn7EA4b8ZuIggqslCmS9LvB2mpdHc%2BJj7AP1p76SYTEzFe8RK%2FKcQB0aMnETGDyd%2F6sxLaUhd3Azh48ja7smyvgr%2BNgMLSPiskGOqUBGydC9RPiYlGNn4hUUaNw7u7ieq0HpGhEX18GX0u7rLinA2PWCb3gF9vKBUuxVKKpdTVYjsRrUpY9B%2B2QaOWlemPSY6g6DwiAUc25pp7OrvqS0R3G7TJqjzWsj3aL%2BupIFO3D0bTTyvqpa0ajwNAyUIXkp0RkJWAuvEI%2FfyPw8kAVFKfX0aa%2F8HFl49ViqWB1ylJBAbAK%2Fp74mJLJaisVsjcmcWSf&X-Amz-Signature=2079029d27a98bf22c7e4ac487894ba44c3c555a877ab4fda056afd44a95920a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

