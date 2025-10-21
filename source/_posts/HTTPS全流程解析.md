---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSFEON32%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T010059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIEiVEbIJ7uBxEG0oFmqaLje38UdN9QTF8%2FBzqnCWTfsxAiEAjhW6n6Dw0FmRR5cczpJgdHfrsu%2FMrzwzo4xPlKqiS2gqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAnGuMumpixMFRz2GSrcAzGgYKSOsOLMbWgtk4iOPeulDV%2F814Jgz2xVFx8Zwn6jAmR8NSX0LFrr5iKytpTpotn%2FKRlis7E21%2FFkd6WB3BkgPVDfFRxRu30b6da5o0pzOsbTWxUoaL9%2B9xks8ePO51RH%2BRaiunTtm8saiU0Yp2h9gjC26xHPtEUFuc6SIymcwkYFbRAyTiYf8uesixMwjZRs0YZPL2KWRBp%2Fh8qeLFOg%2FCdX8WGk9yYyBwLAJxGAhIyQ4%2FQDWxXHJY9IWz9p2RFMI7sLWRU2ei8QDoy3UiT3OB6%2BMOXUAlzQ9rPk4DkC6FZQxmC1d4obyRqvoeuTJYSjaL2GN2qd4yfLd36xqs9yNulq1THxC0nbfnNbnznfHsPnq9W3pwqZkH3XpD90ArErs4O1Mdb2lG3tAsHjeLcb5mVbK5LGkshSW1ZSMRSEUSV7%2FemjZSSL0Gy%2Fjj3c5B0uyEW1Zhdqf9KcUHfKwyrlR4DCxkGxuvys8fm%2Bi4pdeusTfP5m%2BaTIJASzGCUKpxoo922nA8s7gFW%2Fr4G4%2Bt7vT11UaJMRjAN9bCHV9M7UuQOcUuJEVVoqkuTvEmwoYv6dSaGBVSYn8rvSHCuQjYZnkkNXNQnHdI5YlWKplRoxEs5PHcpEdiKkfA55MJCW28cGOqUBF7YhdDepcLwT8D%2B5CtFC9NmmvI5bBgLjGBsEp4n62hQLnkY008r272%2BXiT8dvfT5kRkndMTZHoSN3atsV0SCm42n1uafcaI0OnENwvbnU67MnB9Qw5wm%2BD0qkvuxcSnW%2B7sr8blZGPjQauRyh0Ryq0hW4Yf1zApM5GeUqoIKLtzk6kqy6tpwlt30X%2B6UD9BDyczWD5eQ5L0BQstE72MPkuLP9zeA&X-Amz-Signature=955ca77aa73339982a464154c4ec372c845ef59914ea5f9e8d93ff2044ee187a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

