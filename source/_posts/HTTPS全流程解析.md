---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRYL7M2N%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDk2%2FgcSFCCuw2ZpuIktyKh7gL7fGt4y9nueVaHnC71wgIgMRzIJUtR49VRRjbHaMTQal3CF2cg%2B6Wl8z0jg2uXuYAq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDA1Vhp3P2NY3Qj7RDCrcA6GM4PwlHAGNxS4%2BELFBNbA2GntRZJh8ZxxHiwR4kyuvQnN4OzE%2BHZcIcsYTtGSwCIvk1z6n%2Fz6upuvPzEKsHQncI%2BwinFWNv0tdAH2sOUx6IOQkJzJpxhRhFc2PemyEy4iW7iDCpnBmF%2FEi09AIpyiA6yrcNJLgKvP2Ci8%2FjTx8Tult%2BAly9vYQW0JRh6ntLmmbd7KYXX3LCHMsGl7VLBd3XUhzJxj7st6O51f2f1RYSRmNfTw2XVoMkf2GAkr09jkLRSYQxGBJ4uWNamkNsC41NdPcB2ZgE8hQCdyoiGKr25kN8d2da4edOhlfFyc567D0rtmbo5XJD9eECL1GEeAnSh1Lgzchurj9dZBzknNcjPO9MK0kRhdd0AgwHCaZc0DM0BP%2BKoVczion4DrzkKgUdDuEcXeiAZKHJA%2BEizGe7t7cVdAjBF%2Ba3Urt5fVBVtskbJHeXT2xzY3%2BpQCTBa29I2kNfeGqEyi6vUavzHr6CcTTvpOZNg5WoQ2ObS4%2FwME653RBws04sT1QXTLAStXCb2%2BaedZ0fk%2BSVzOrVLzWytChC4vLA7mcCeWZUZxMxKAUni0SuL%2BRL9udeViEs8W6fCy02uLSaf5lfkehOrTZZkeHDL%2Fnw5JYnn3wMNPJ8ccGOqUB1z22EQZ9nMKHyuOfY7sZ8C4txL1%2F3sQWZROHBTx5Eego828W0bmR%2FLEp6z7tfVPDCBDEMVPXfiyq7ta6fyhziA%2FN7XLz7itEH6%2FxbZplMlfNctSHKSJyYAfmIdYYkJwnuaMISK8h0WCgoJpkLdqp0Qt9xtu%2FY%2FTNzLa6ookefz6%2BGUB4ASMOr7Hwbg4%2B2Gc7lHlb%2B9a8thZHcnXV8XiyEqXMnXF7&X-Amz-Signature=6b8c5683601b82d664e8f7dcca2dd29f5cbde49d85f64c13753274db8d350f04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

