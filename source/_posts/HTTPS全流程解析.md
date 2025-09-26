---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UO66UESM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQDk7G3%2BUoufCCfDkEXvL13PCL0UvYskAAHiEO0dCiRCqgIhAIEpgj3no9m4oDVwxw8ROhyR3q9E%2FCCQWjLKDeiNsMSoKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwz%2BI1T7U%2FaRCJfWKAq3ANVRWXgw9dNKBPT1eyvVu28%2FhF4uJdPtRPCKOS0uncIkRvfXfBkJV3aEeAJpf%2FFtVU7IfC%2FM9eFbuFepFp7LWSLsqq7Pn5cJ4qi33AwivSRd%2F0TcXLJtv4WkNeEkFpSQJ5i%2FDVUdp%2BDi9BDTLA8vjb3KYM7CmT3Ocmy6SFE1LG3G4EgpmJ4mrFdXFLiI6sZcFHpw4062x3mh4UqwhZTlouAOYVQSh3bAnaM4dDBN3jt447lzhlJ1p4%2FqcGqZwc%2FxvQuoBvB7vSIIJv9Hu94JDQ%2FudHp3E1O9FejeUw2WlGacBTGAfMv%2F1hnI0K6V%2FGiEjK7N8ZN8CbNsXgxeDZkPEdCIc8cSv2K4%2BDeufVbDnv9dfSOLCi4%2FXBCreXnjLVhDBLgA86vU1kXaeamByLWGKAWH2HtjNAhYspRE8XH5GbFdev2e83JIB1Pv%2F%2BhOprJDyhRGpuKHcCHwDtf7LF%2Fi3syD6X6LblvocxEoeP0qThYj0j5Nw28%2FnJpafAGgkdinq5Won6WLQ5WyP6bT%2FyzqQMo%2FguN4c0PGMAhhaqJP3q4UFNKLHtP0wsVnWHSqNhNAE2fybHbeIcHzVFgZMFlsi9xcNqzB3pJY4C2UDmmeNZuIs0vfY%2B9Tu85kcm6szDC%2BdnGBjqkAX0jVr0C2wIPl7%2FYgELKFp6GlFr9vJyPyD5g9geNG4rSgJ6g8ql9VIJvYT2g0Dy1ElPKb9zKI9iXmWtu66Q0Gymhe3ZavQ8qCo60oKZAjCoA0Ei29IDD8lBg4v0ZfyxpTO5%2BhEsm3T3bg98HoJt94D5C24VeXgf7ezsgTZhkHi%2Fe%2FSOqKknwgL5mudK4FU4vTNusC1ST1xGW13uloBqgPatDVYkJ&X-Amz-Signature=1edc33352c8b7f63b2ef58c2cf3d5fe4b58921be2b19ea9a94962698ed589896&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

