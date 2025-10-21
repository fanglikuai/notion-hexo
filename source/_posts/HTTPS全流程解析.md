---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRX2IQVU%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJIMEYCIQCAqsON28iguTAZwTI62oRJp32oWltzNz6eViKLw994rwIhALMlmlKK8qPlxqPTMhFMlmZK9Ugie2w3mHNPwUwLHGs1Kv8DCBoQABoMNjM3NDIzMTgzODA1IgzgMLgh4b0L9K%2BlpZ0q3ANlIjeu5nuq6Vl3MgD7qfHejnOopQ%2BZy0cY6eHOhY7bqLaU1%2FxbzccJ7nnW%2FVjw%2FTsNAQxC%2BdzLsK3EgH4cMiNBxn5dQwfvp5lRql40mdwTydxwz1WNX2rybsYjPImD46qLZsovDOVZPf1exr41iJ6aQqWLvzi0j4fBBw5as%2B70zUP2EYLtTSAq6hu8FRcKaqy4ti7pf4zixU4udYXyvauuqudy1Mbmvk4LzWGJ4LkVjA%2Bbjhbe6l0epAz6e00IFF4rE7PSJ2i8gunQjO7AUwbYA6R0T7j9pfUp2AmysVAYzGy2liQe2U1xBOvK4JR3faQzxWOiOGgMX6LWBRbblNjqVObUZnoh66GFwvPjGtRASvSeUSj6SOXPCy6x2BGc%2F4SOza320zTsNI4Ixx%2FTz4c%2FieVn7b2skbCZIM7%2FX8NqHpAJI%2FV2r%2BBAE9LdxTBQpwHhUXCfJ1AmpF8QohpuE73Gqu%2FsBvFObZ2yxceba8dpjUYr80N%2BpFTKT4oZMaTfstyXUbWWnrY5SOeuFS9pjJiWSC5Dp9Uv19y30oL3lMkrsjllUFM%2Bkk4x4Zq9Wm3B%2FfHnJCmjjIDbob0NdvToqMTHRLB64Bepe275HFK9CtodvYIQbAOOPMZOmHFP3TDj897HBjqkAS%2B1BspavBnClnJ%2BMq4OwXZ%2FPC5b8Nx5aNSlzoa4WoS9OGDv53QQxVEG6Cggqm5sjRwv21uROrg4hD2qd0semKJZimQeYjnNEKBOUKwAv0yw4OXcMJLABSB6xGTgBaCa8T8iH8tfdQAXlRQspVxWNJSuO%2BjcY3Oy7RIy3D62G3qxoHqoKles1uVcslGZNgjGIVnH8%2BxTaRfDB6UG7wZXdqnu46gT&X-Amz-Signature=58a10ae3f25e4029c9f8c5d3d266439eee89f298f170d7768a0379d5f8708389&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

