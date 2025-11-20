---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZSTALEX%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQDabK0AbR0Pq52f5r50nMBrDrgZUts7KOGwPWL%2F2Wlj%2BQIhAKeewh9gmLFRrN21sJzoKA6m9LJR9m762%2BENKOe83KBBKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz27TjCEWNNiAufud8q3ANPCAaAhfvFSSKR4A8Q7CPMx%2FF%2BcLZaI9sxBpjJPKki1cgB3znmGJrKAQROQo4iwS%2B%2B7b%2Fgt4%2BN6lCq%2FbjB4QpgGfdZj449Xb8BAxdvACP023beipo8SA3x3mWYdb5PcNoSknpub%2B0MDaDQ0XQ10nO9Nr9xu9kzq7MiMDLcr%2BAJH6rUH4aMCZQAZn%2B3deoRBLDTTG9x8M0QCwxJKZ59FXQFopBg9qemaIMjeuH9%2BB3qGt0ZI1zqBn2MFh2VGJ3iAis8Ku3XU10l8WOngbfKCBd604gAY7sIHrEU4LLQu0j6Do0Dfp6mQ0e7ys0Yh2mvr9HVIT1FuLthTRPC1cGY%2BMUI%2FW%2Fi6Hp%2B4vmtzVVDY4KsyuPV2VGyMwFavWX5jkJBVeT%2BCb2NFZtwUA0Nl3zClwv7YKDKqjkvyXR9yX9FG1Yg4rMbcI8KWpL5ADFwWIw%2FBZOGDBWH6zzbEe%2B2K5eX9xuZV%2BxuNiGvgrhKtcDsQC1xfa6xEyB505wxfsfWBjnQ6YrLHBZfdQ5SOxcxWkFMzgidCQkCZcJtFMg%2Byx9QmRPz9o0hJP7WbZ7nBhaRgokDtMYYiMVvsVQ8%2BbqzBm7v%2BUCZl2m6ZOWvKR%2Bl31uyYWfxOMZHIZBQ0dVDk7R3SDC6xPzIBjqkAS2ahj0KKDOeJhEGtPbv2y%2F7%2B7VhmT%2BXcTO8CQP9CFgcyjcmAsmca9fHqAroJxFqa7QCKwnHwA6ixjNG%2Ft4hf2YWf3x6%2FpGHBha1J40mfE5w41UiT%2F8Yu9KSGPyZBbR8Q9dBfdNY1LlsWzGAsQNNOm1qqKgp2tEtj85bRlLl6uW%2FpOGnWJECRcvZOHHFkltPgw0iKqcz9RpeMEE9SpSUe2N1scm5&X-Amz-Signature=5153c1780aed9a16d4297a39db8a7a672d6f8ae2211cd8dbbfba15eabc3a2619&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

