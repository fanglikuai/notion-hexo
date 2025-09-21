---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662XDV2HVX%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICiA9k5quzX%2FOumdcLqbPnXhRABpK8oCF59kjvIpKtwEAiEAx1alc9VjtM3yfpVlYMu0umop0avYTbPJ9ILk8JagbKYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJRHd3Axr7gV7Vig7SrcAwnnbooe5PDPU2ZFdDMRAdv6nK4%2FbVwEaeN8yo9ZzPPIbUR2cE%2Ffv4f2r%2BT93BXIhjm2JvuM3P8j6rU6xbBOPJUyNZpA%2BObSGWzHLpjc1RQpE0mjQm04D%2Frr8r8wT8usC4riN2WdlY00eB2aF5L2Qsx0uBgBNkzcKdiKm%2B2YNEQX5tX%2BHla4RdhxXzz%2B7eZW5hXUQ3lK8qdikSMwWnzWcogZhP%2FKx0avQIN7nBNmKN9Kz7iQK2b%2BwNHGO5oUWllPBM4zzCRhwjSNJzMdieQnK6Ud%2FRp0cACUTH6pO1QUe%2Fkn6%2FFRBHjGGyi7u1ssvk%2Bh%2B%2FldkDqdEnX7q%2B3hkRi%2FGpJ6%2FdAubIG8Eq%2FLsknwptS7hGkAaiRJO4OaMEg8fquxiAaTl%2BQUsj%2FyZ6AfnHzHNVIbEfv6KpGxYy0icXb99ZgB83gv2KgWo1NANJunjJ7bN4AFLfZPPFHrV%2Bk0%2BRuSP0AqWPDwELsDSiGCFj9nw%2BB3RJ10vRhsRq0uGlQaHll7muxy%2FNa2D3vvZeYWGc3wVfkhTskN2mRToDLysJd1ZjIpfJ%2B15lXR6ni1H50sB%2FlxJQIF0D9Mi4o7MBMoOHppR3Z2U0kZQz61BueiNy6XqBKjnoFQJr8wXV6MewGpMOP%2BvcYGOqUBJzArr%2Bu7ZEPzVS0CbtdjxoBujngQ6Vjwtxj118O%2BiLRM8IKmupvYcKb3Fnb9NEnDIWFASLXZVcW3GDMNJvYx5BpQqFR9ThfsC7JtMYIozrcxUWh9SrPmHENNkoOQtjT9ZmLUNP%2B0wq36l7b89diLgxFQBHpjYPfN7T1pKudX%2F6L2U%2FDXJz10zfUvajErKSv9L3MF7du%2Bo6Ayv7EBEShu1eaumRpV&X-Amz-Signature=105354f5dee033d7bc85fba4046b72559d294ceef31da883b3c4b61608ccba79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

