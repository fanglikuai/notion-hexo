---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SSUHLRY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2AMWoPR%2Fx3r8T5ZDY8qTPnq0I7yZxtID2vpbOwCwWbQIhAIctPpLfVh8m02FtbZeKmvYYXXl35DoU19q0a%2FVTuRi%2BKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz9O5ZqZ9BFw9yVgNMq3AP6muGcLFOQwJ3sP47Xke2xzoA9hCvMITa35hcPzYzjftUYQCoQ1fKoWDmHnOBYaDtAWZeg25p%2Fzz%2B5Tew1c%2F92IW3AsFQlJTY3sZdGGSLhpASIvP6jLjhZJ5od1gwfCBBGLe94mVUIRs%2F18Z5bl4Nhc5YDATOC%2F7QQ5Pbx5TmcWPbk%2Blo5KhEyOi1gL9sLE%2BlwjswPdIozjUmhMQB3VUXBwpbDMa%2B23KP558MYKIYnp20o%2FXUUj3uICqeQdIVPVszfFYdxHz4CNaX%2BAkVABXNjTygvzieTM7QfDhTV2Ngza7%2BCw14Pgiib06Vj2tIgWe5OOQaKaRM3Km32JizPuo2phBDz1DO0emM7VWVMYui6CyxOSwznhhW7nqGPefgetnWW8a3ISmlcr%2BqeO5p0NqiAAJO8ao4SAvVXPC1JCg6%2BmDb2nGv4o0N59QeO2sL7KiqB37PswE08mmmKkfzgbl%2BPN7VQKQtjALD%2BK1%2F83YEf7YC26u3pBQRmtJhKOhEs7GLc4BHWuj6UKFizZB72blSf3JlomiQkWSRSc670%2BsQnDPcFxLlSpLC7R5Kk6kb2A5ZpawKbPemTPALoV4qv50DuQaeqsuffDQ%2Bive%2BaixTxE8FqR2SwcPtX3MAnOTD%2BqJ3JBjqkActYvwaWKcwCVYzl1AJFBs8S1gmCOjCyHyCB5BjAKR6EIH8n4gUu1GjZHmblhRoLdFW98IQUDm1x2PgmKqfMIY1OdmIuGaF6d4zNklBvgVPhpB3cr4wVkSg56hB55OABWwiTipcCsqf06jP8wdYDuGB7jcIgXAurzzs4GxWfGoMcQ4UomxZYRpM7xAgph2C9VotcdyuzedXKpVk%2BpZp5D5iuIZzs&X-Amz-Signature=60eeb83d99aae00bd33b7f94aa0f954192def9b8e27d33dbf3d16bb17ded376f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

