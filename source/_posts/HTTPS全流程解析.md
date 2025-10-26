---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HLOLDG6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3Kec986VIJVmtWKP5%2FkYEKiuk28MxMo6pYkyOIjACTAIgcSc8qD1g4OJga9ZGqWAkc5GtG8Agv%2F3Yt%2Fuy3SRqKmwqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBdDDazDTadm1ZES9SrcAwBan2GrlXOd8g7vjilTSi1SaHivf9rHnwNploAbB%2B%2FXHUqtaSPaLaXvBXkgU1AAgzT8FeNzpqUzLYhZWlIyaKA1sQRuK6Ty63d7kgQ3Y0pMfZ7QeOYlW6dm3tBWEHONU46U8BiNaSNZwGKovwuS6uvts2dJDmge1WF218VxWHbv8b9iO0gkZEUhiJaCi%2FSCMn59n1Piiyh1S6Wir9J0AV05Na39UJWnJqGwjKdDSm8l6Ab6T1UIexFv8XHTtvB92KqXNxB0HpKJA89%2BzDsjKpIMz25HEcdg7MSFUuNmS001bjINBRZprNHPP3sYV0DW4U6pfinhZWRLhz35tDEgT%2FYgoj7yfYizMWM6C3b6HotJ3VjbWi2MOLN%2F10xduBLal2st7Wk8FmUyh4RV8RCpkwYXw9oWBp2sY6mtxbEP6%2B1tBQYEb9c5l0IOMhP1IXfmfiYFgx1RtNwvKTeGzInUsCgoFNwGcg0DtgwMDhQMfnxa48v%2F85HZ%2BTK%2FyM8WTz5Wkp6AiLHySHrbyjzq%2FsCs%2Bobr%2FCX3lT81EW7cr5aQQ70gvGDaNM9Pw5Ns%2Bo7EBT9%2FpD%2FFsAXVpnE90zJ4qcaFZfddPC7x6EW4V7tjlGT7a%2B6Xp826sPKs1LmXneeiMIjw9ccGOqUBPb42B%2Bn4EPJvMIybp5v26UNo2uehS3w9ACOSstchM6nwlzkd%2BBRPfBwsFTE7l8pfLAbMZgOKvVOQpm0ojm2wlg0wze0ZK4Ve8WQCTiJ8vF1V%2FQHg7F00E0OiCKW8PVnIl%2FaxQLvYvEHumQc8ivGSurRFUc8Bn1Lw3eNHlK6KkhJI5PUECBUIyEbEHO8pC41lTu%2BluXX3bv6%2Btsv%2B1Bv4Hs3Lohw5&X-Amz-Signature=ed59ab4be1a635b41216bcb2948564e7530077a7b5499c4f2c231967ce076178&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

