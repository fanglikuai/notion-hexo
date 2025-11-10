---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRGLFFHX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIEYYjy0cXdW%2BEMXpvA3sJSceknnvfnQOZMG0IMOLaGIiAiEA0UAo4nkWST4iOj1zTkzT1oH28s7fazG8SuB%2B1Arh9cYq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDBQFIKQ06QxYFHTzHircA0261kYKW%2FVx5HvSddeGLu%2BHRXQRzPGgVeS6TbSIUmVSf12UklR2o9uMnGw3hNAi5z8w3g65HDFWmqbXWQh%2FbzdW4POH9R1dErXk9m9dFySLci7qTj5RoPUtO7KEQtyHqGvtXW%2BFzGYvnvLHLAH4vlpoSNwUYPry4yDFzYfe5XCrjrYDodYhBYm2988w0YuxD3vzAHWcOET9tr4qx5J0QXStzTiB9hgBOeCDEX2nd9L33j3tj9f7jMlu1NpQk%2FU%2FbBHppXfYC7eZWWiZ%2BklGa72ueik9iAgZ3uV%2FYTOV7hk3QVTJjXORRRxN0Z55BHaSZ%2BKn9AYwIEQ44MF%2Fy4YmmgXxvllpnbthHh9h23KUIk%2BO9SV9h4%2Bz9De0xa5xqfFIilePoaSMQbb3ZOaNAGhDJEr2tBSGi1uPsvcHsFrbFqKJaF9LyD%2F1X8V6PfCY6C68f3PnG0XKF8qYkHwGdD5%2B8Sc5TOZONGSIkGJyORTY59Uh%2BPg7MrsyrfRuftq2%2FmtO9AZQsDigYCLTTO4BmM0l9J0LK8u9amE6Wd%2B9RHhreTcuCbF825hbp8tCYdylUNXgT0p7Y115M6JwUG5OL52kW%2BN9Sopkq1EP6tklzGBd63j3obrPY2q9Rb4PUIluML22x8gGOqUBxlWv%2Bojxl5fwBNHSvqAIJUzzQm0zPpzpZwt5JfaxkeIrNKJnQqeSYCcX7Y76SXSUEbUbBHt3D6bdKlQsUNc7ZM8K3DaiK36tK7xUJ56lH%2FdTEdAcBCEObQsPaPAKa%2BaY0M%2FlJ9TLuYzPmIGWap3o9hTam1HuhZ03BPO8WVbrNEkSe2uBBXZjE8LDox4%2FwFhYdShK57yUTT0BX37iNp6X%2F3GDN%2Bwg&X-Amz-Signature=f745eb8401780cb47c3068f661d8377eb263f483adb18f1a4fd3c0b4fd372dfc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

