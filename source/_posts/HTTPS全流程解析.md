---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Z7NKKGY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIGBvRQTFT2e2nv2Aiefz8EBwLFNzsG8n9mTobK4msEq4AiEA7B4p8PrnETzR%2F%2Bq3K1dQKGpHTfz7ZtybCskWBcfd4DUqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKYJP8BSu%2F0e9uzXCrcA9iMdH%2FCFk7YL8pmVpD%2FudfIo%2Fe3gbT9bOI4yInPi1oJoiSAHg0RDCc95cuORd58qRs7NFtCnofqIfKofvRKvwcNfqmfcbdu6%2FTzi1DGDr5A76aVfD72AUiW4WO3K%2ByNXoroy6argxMVQt5CKj43R7%2BB4ww%2B8ThYEUTFMz%2BQlxKYtFmdsb1a%2FRrDUvhMZ%2FB8D5QbLEfTHBEkir53fX7I1LaiteduTR4ee5EHWp4Av1GodD7pC7BvKsxcH2ajJ414R%2F01BDpYmyOw%2Bh9uKfu%2FJZHFn%2BIQ7HvfbDDKuJLe5RHt7uv8g2zJZiA%2Bhw%2BBdygLLYTr5KZpzy4R0anZPglRmr0eKWutBwf%2BT7sc8leJHja4iR6nB%2B4BqO7akA5JEuauZ%2Bl72sZtaZ7wjAQSOjk5HzGSd0wjMeT0zx7ydubV9yzwXv7ylyl4AN%2B1pGS2nqKn7Q1DsdmUnzH7Mfj39M0FrYgrx8u9wLSVKfXpVrNkL%2FgsKB0RnH%2BheLhbyRF4XsE768zFixp6hTuIrbUynHFa1WaJCemr8NVGqiXDTPKQ382svAf1%2BOxl1drJ3cz5eieqPRz2W02FnIC0FCh64yKNoMZgIr%2FrjAHvFXBc5AU7B%2FIfpK3RGQUJ5MBRj0z1MMbY8MYGOqUBZjSeTDJImOJ6gHeXHChAntw2DCPwUL2%2BYq02GOkb7uYxp2uEcVJs042gRbeaZzMeZZlwnZk3XiVfvCDaMv11GVzTpB8mob9aLxwVCi7omeLMf8y7WGInYaGtN5KyJ9u1mm3vczROJBPXfgFfH4F5U6loq1dUohZte1BCs%2BxWPH80c3d2W%2BOIEAQRgCKtSvgIDDRVbmrPkKNV4xVxCPByo6J1t%2BRR&X-Amz-Signature=bfabe64e1a0606e966643c572d5455e70a80a96dbc8f1457a01a3069a9229104&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

