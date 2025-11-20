---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466257HR2OU%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDBZn%2B6Y4AkaYU3oEYVt8eEv30LDPfg8QJVfZWBKqqkVQIhAMG2PWT2rJLZgVGj9y0ZntKW2Y%2Bn%2FOlz2zUAO0KxetvHKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwPZhvN1tpZhfmkDCcq3AOivPze42m1CWuYnRYkzjnpUCvWqIOU02S1R6kuG0KFnEB1UCkL6KeqGMdTv7VlvsRZXli%2FM4277teduio7nRFDL64am9FJILPHDhT9prQcLjaxw8FprH75JLQXQ9scQJzC8%2F3hDcPAlb9WhjRxkd8RVV%2ByO2UCrsiUyVsl60%2BrNDECtALVlESptvpBbnXHyXAUmo%2FzSkQkO66hq3yOYoZDtEBZ7YqbH4gYaJmOHAoxtb8RyExj2MgSjS1v6k%2FuqzQAXttfoTdaHnbp4Q8SUGZYxAWiGhtGoedhjaIRdOsJo8t2LR8LS9r5LF4g5xqbbZHhTKhR%2BQoKu0cT4Yn%2F6LoURKbXYDIC8uszV%2FdFgIF86OmtOau9B4dR9525AAXfqs6oD1LtOIDyzqCLHW542aI2GM1hXXgqNGWDkWuUiCNlyeNBl6CyiDcCvDtWDP%2BSK%2BnIYqttP617svX6W1X529MWXwFIutO3Ou320kWLRHAoBl8%2FqqZkWpXOQ0x%2F%2FatkRZ%2BjbZdOrhlc1bHz0Bo0zxcBbQ9AcB9ipzf750jARYcso74y0iyx4uGT5DTCTp14EWF6Gjm3ZxrOrWOulIEXCUZOIpzq54WiAWBQWLOElScYTUfSF7yB6kERlw37mjD8s%2FvIBjqkAZ7%2FJGPiGurSMgYJZQi04ftvAARrbc9uoL%2B%2BH6R3v0RMfTczAPso42CIjPXM9uYCGNiyBOS4Hjol2JUBrcYI%2BbgKm2XfODuWfubxBgQx31hetacU1V5L8QeCWDWVMDUHtQxUJ9fD4%2BZPUiS5ZmK4DRHVJRii8Ao8wfTWsTaPZLDwBJQx2BCVym8Fe1AEGPVr7nMpeN%2FfurWDFaf0No2BzbrdwLLA&X-Amz-Signature=99f6294631cbc15707cc73eb80e4eb9ef2d5a22c08993241c927fe8957385bdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

