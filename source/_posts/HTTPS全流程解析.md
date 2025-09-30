---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622JHCDKA%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T020517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIH61Y%2BNPj8zuXptWap9WVHLlvQCwqIZmP6Ab0tSFcbLGAiEAp%2BW1ge55KBQOxvDtJUZU9P%2BrS6mt515QsN7%2FAiw6NRcqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDPIcbxDhnus6ZYafCrcA60d7UjZ5WgRGElV1tobTkFD1B8LH38vSTDwnv6h6dsB5Cy%2FFC4umySoSJw4UJZGFQO9xlAw5s8fdToBabZUyyH6jSxyoqxadQBkwxKU4XZL%2BzAe2eZz03R3VuxibX%2FpRQaDJBRFRHjln7pNlfC5IkqCnb5Lv9FWAyDhTbZozXL6ASBOyAVuZVsqQFSr5DrDRqydk9KBe%2FuoIQFHpRSXjxdTNg1WM3BAPvynjdQpUNoc2xj0LHnvTLCk1Hdc0L3Mf54EqdFvqn%2BCyjgqWWeYPVhkuwiOrDNQSnTT%2Fwgiqmhgy5jicELlINVRGBI%2FymreXL1eHT0JS1mwHgfiOuNwviShozgKwAdEXwdmr1VLbY1HqwEUKGqu0v32j9FwTE54I%2FNaWmy7REV7cwfjUwy1tTooDWYmoHEwAWG26qoeR6b3I%2BZk107%2FYOgr6%2FcYq22zeOPI1Thab8cpq01GUDel9nQ7d2n17xlDC6qK91M4%2B4o2dSWD1%2F8dHlPllBz%2F5n374BH4p3%2B3SGcCAohnhaB8wW%2FC%2FDaHvTLCfsT%2BQg%2FCnWpK4LeGOVE6mCnE80Yuq%2B5gJtZB%2BpxKTDT1vXM49kWLtK931xWxDr7nuvitdYJ6sb41buZQSjRvua8ysrVNMOrn7MYGOqUBPV2LMu6qzl9LLvx6VVLtzwtY3lfayPm%2FJmJ%2FfrHJOv%2FvPKxJdu7JqHlp5XQlCse5IdJjq5NLDXwtU4sSosCKqB7l8OGMQGI9Ah9JjBUvny1YJGQb%2Fq6NbddXtYf69AmN4qJF3LHmZcU7PkgfV1%2Bbf5ZHQGAB%2BLdfpo7i8jXtdycfthuuxXtWDIVN%2Bal%2F8Pl2GewdG58n5mlZlsgDbZrRp4cp1hA2&X-Amz-Signature=80f611e688343d64d04b619cbb929337fc682390ed1916b63eed4f8bb3d774b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

