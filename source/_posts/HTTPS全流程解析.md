---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMD3VSCC%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCicdwijO12OUC0rEB4tT1Et7aJN7pv5z83OBOlW7eO6QIhAIgG1wV9s5oaCrbz4OCOU6gwyYFhBh2lWfCmIDVcf9zyKv8DCHAQABoMNjM3NDIzMTgzODA1IgyTWgHYwj1Ep5HXFJIq3AMuBK6Whv5xlS8aL3nqxePbppettX4CL9u8Req5XTvaw6YUBsiVbZ9U%2Bcc24TFywB338HKaJfpM2yR5Aykee0DbFutbsVJG3eVywZcXwQtGlEVyl1uJ8rGBwRaLOZ9Kv%2FXbaSPKpgXKLHTcAxWrFJPT9lXL5o5Rhuiv0jhlhTMGYozfnfeVFXcoDffWpsAPfGsLqzRh7LGj5fPN7I3XC4xmIZ4e%2BrtHLvflIYzL1uoNq0qUzoS8Q75SxACbIv5F79DMrusVDnK5mDtFJL%2Fi8%2FbM%2BqVUueYAclgJi25xUjrFXeKHYnZEBM8IwgUU5tBpU4EXbFdbp%2BxifEB%2BzNsnmgxwDF9VLILtDBoziKlmdbR45Po5fNfmtF3E9iuRPzqO1Zu8rXs56%2BN%2BLGscDktjaGFlqg0UOBo2MUXwa%2FC9PcZP92iWC7kNpfH7iuayXKp%2FTotRqDnHTw5ZgtwTLcnaQvXiabxjIrluSWO7UUGH5Ok9iHB1FKwF7U6T4BeAgEUIlhJFV2Nmd9Datt2g6XQKX74AioKrvbrdlELVCSPwxfrn6A%2Fnv8Zf3rpSp4Ge%2Bj7LWoCb7OrwOb6%2FpiE%2FruWVl7jhgbOShcdaBPu1C2qIpTYANMjwY%2F%2FEjxXkedRIRzDMy6bIBjqkAXdwejH5m%2FHkRtsjil3kuK3mmgIcPRnqMlGUvxlnDgrCvMmM8OqesihVUZqGBrmyBiyR1FKE%2FuXR%2FEAfvncifKC%2FBTnwrYJIAI5Y1O1NISDW%2BKqu7kEpz583yPpTgNbNJW3C5g9fPYuu4HoSnM%2BA%2FKf2d5SwCKypiirAr7AKzI5xbiDN4O5Vm%2FegvKGztH%2FH6jvyt7a2gWtywfANWJQY9CADEsU8&X-Amz-Signature=c503919d15f75dd153d229fc06cf6b6ed571a68ff24e26fc9100c8cab103b022&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

