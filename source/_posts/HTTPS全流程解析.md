---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664JX2H7HO%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAdTTRSDqzavtnuKYm%2FKmAdfw6ul2ayrGcoo2XLUeD%2FbAiAVTpS1jUE4kTp4qoqev0MxG5jsZ8xewHOGKO1chLzisCqIBAjq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwRMy26Zw7%2FJn6rE9KtwDjJmCvqpjxSB4ngzYHPz6imY7kAeH9%2BtAOygA1G%2F4kyec5XFG%2F5Y8RfhbtpW%2BTsZxPYNHodSaQUDKq003Uh3WtKK40oizDReTLA7yaXF7wqg8k6VZ68psX95h6jHvRSYp%2Bbqd%2Fe%2FZ%2B4qFujS2O8n%2F3DgWlcK44rMYq8dRBgK0IIx6FdeTJ0kQhimxvngV5O4VHDaeiJ7hTwNKK%2F8HaoD0ZIUZHnjHhQySwjyfUgdPWQQfgFTmMLXjcTExJc%2B%2BkIcmDcVZS0x5reWp1%2FlpO5qIlBGG2%2FSxyOGvUVrPc2FxMLS5MUvzrJt2b2awV3760X%2F8fHORMWw5jKepLptP62MJsWlCMWsM5TErXq4qCqwuuP%2BAYkDvrcYJ6TbmCwbpqybS3tY0g6Q0KuwAus1iaZLIpuOeY4rEiIyBbWXlToFdxAWS51uYAPN5oJj6Zklh7zTdpzcVCIoccR8oob2066%2BbdJENowaLS6Yh%2FHCC9GV4exdovqkACY%2B3UirCx4n8YmueOYcuKhO0qrCWkyXR1oGRPjwa8PrXm0eNwS939I3R2mdo9ivFMYQRHIRB69UKiM4D2U%2BkhuKAGAYhCsmM8e5CiW2WV4VxACALg%2BHBV0Y9CNWovIy5H1MZhnq%2Fu0owxNX5yAY6pgFcu5WLi3XqzZpCwnI7vnRrMXfPVFffUBJ8NrF7UHWqzP4dHVI0XOaSvnUWjM0r6ry5XrxylirnQXPetAOziDSqqO4KduI5Y%2FkznTo1pyGp8ziOel0V7ZZEe4xpDbTI8FWTe1La%2FoevrGTQEOB9US9M4QwXnucx245wD3Dw%2F4Z1ptny6noSNsM4olpsZLyj7k5kS89xuRCwTDWFKkWFEhYNr4QeQyB7&X-Amz-Signature=3396d9cd67ff97472e7adfc94b63daaec07960d527964ffe56864d5e87f8f7c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

