---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJT5V4X6%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIDSDX8Qx0NfGlxRU9l%2FYdnHy6LJgZN5gS%2FkgT6tnDpPeAiEAno5iVC5DJhmjodEMR4tJo%2Bo8OLqdZMJl8ijLpfi61K4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNP92QMvBwKAmzpVsyrcA2kRXNuzINvsERmsmwpZwW1o34vHqjnhv3J5rNbLQsKX%2FPiAGSug3ZZsIWoLcakkh%2B6nxn1SMGevZyvDEELzvvQtQMSkyeBHqk4RMAtPaK1k3dY%2FTikGcQp5wJyXwf3VoR6NUWQbuJwGMYTW2CrLGuup5%2F2YoX6LBHLb9x5rMfmCs%2F5HvURaO3TQlVEr7xUaSEPfmitPT%2FKs5WP3VE4CwQ7eH3MPYDCIcKFgzEki%2Fm2Vz98XJGkxJhH56lI9bJ3kzBe3ARNs6ipRKSbdxrMk0oOpBtx70%2FzE%2F2DrU9WqmfC92N8jVa9CUCL5j6Tpme6BeQOYvpMfZN90y08QopKbd6GgtrOPIfQ5%2BewVieeDDN3MSs7%2FgKvjbp0uu2y99OGHvZLHCxTjOMW3QIGi4zODLu%2Ftkll%2BvOrgOf2o%2FlE9ZRMpYlKRR7Yh1IbiGzs259ihPI5P3w1tnSOnJ33ww%2FxNkkFtlQBtnCgLT%2FirkSF92Qkipv2c7By1e6c7Z6Y64g85irVbYCn2ot4%2Fu6xDjVvPPEDJPNOPTlrbbHpKjRrBLv3cq8Aoti6jAOHYCPrGUP8%2F%2BJNFo5TVjDXftmVEz31YR1fJBGJUmE1Q41EbmVCROpEJQjR%2FNnK5aXnm0DG5MIXouMYGOqUB1Lu1PRXX%2FPXeCEKLV94G7SprnmQBq5lJyy3PAtkGXsaRIfMtfoZhws1LaT9upvKGkj4v5QVVofYPwPq4xPutvtM7rCyZC7TMmJQDGeXVDSrvFHWblHpsnwkjKWNrwXPMKv0YcRAL%2FFZYUTaEjkSWDhO950%2FdeYkAv3fiGVeMWj0g82LqTwtbHX7m6loEeIDVDWw4crQFKLm4%2B%2BGkryCJS%2FsX7Tve&X-Amz-Signature=7ebac02b367aaad7c05f5886f4f394c4906d4f36fc9dc984b458cedd87d7f569&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

