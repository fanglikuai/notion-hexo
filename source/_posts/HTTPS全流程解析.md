---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637UPZEIP%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWLP6O3KOc%2FNlN0zLtZsaOUEODTyN6lnmU8m43n%2BWRfwIhAJ2kSq%2B1r87FD5bW6cGPik7r719iiN2KHhuaJkbtUhbWKv8DCEEQABoMNjM3NDIzMTgzODA1IgypUP9f6tnTBnvuFHQq3ANFMtck5jrTqnqHeprDwDUeTMReJJv2PQFxSdgKLHuZ7i5I8fwzBLzBp3viIpcOxCQ6GzY3KJ%2FyLAbjqmkHeAVhcQtor0Thk60shYvV7Q7OJkgF%2Bo465kz5IALzvqFwXvm8nKU9kBwCW%2FWiMbBUPepDq80gHx71nLasmHIq3tRG%2BbI0TfeuyCOb1GjM%2BeTH0NwuFpb%2F7t0zp61RjHrR9R%2F3BPV%2B2YPxfX6S9%2BXSokXinpJDe0nlRHtiBU4L9UFqAiT%2B7BlQbjGlmTke9wQvxDW5TZw1qnSzT9D158BSGOYTrUxmGrwpV99FVhOVp%2B%2F%2BvkTQSNnR4%2FBA2bZhYxzqTUfbuWTMxbR98s48PT6nLLPw8EHfu9DzjUTSuWqLz1htWZUaalRQredArsoWLtVlE8DQJ3%2F28hu3ajWDc4Qyg%2FBzvdhKcz6EhOICcO3bDN5RyOP3UjBuOsNLiqMF%2F6zyfMq5nOCbUqryz5AgRwUHnW0K1PMWqmuhbIgdZ25zLaSMnhzcue8d7N7KmB6XPSrLNXLGV%2Bu7Pgn4EG6x0RtmdBgd6m%2FwJNu6weN2mooCZB%2FowWOiNVIePSVEOy%2BQm5TB64jq2UvcTWwLwSZz8DsGCjiV88wq99VTPsegby2sTTCT4bLHBjqkAdW52geXZmLwwKX27L3mQlT8r%2B4SFmNzgTQzHcNFBbZF%2BECzTiBlwoSh0Kty%2Bny0AcyzeaKeRDMIzHLcMsndgk0tW1UiNp7jBfiQaspVQsWHvFqE6D5zuiYU6n3QnRAL3f0ix25IZmKO7vITLDxPQpaPc%2FignOlCEPM3IocyoKjIY6CK2go3ZdGnpko98RQAYPeDoYLOk43d%2BAATa80AzI0aIfQd&X-Amz-Signature=7c3e7b2829f00fc6b449e4e36163372b3f49914b6e505e16a87dcad4dfc86ddd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

