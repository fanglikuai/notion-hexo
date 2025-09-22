---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MUZQGHQ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBjgCp84ZQ4AkqnHFAZdw2QBCdbkazOPuLtg2uIOzMQwIhAMk3Qb8aFoh3jVsQ4NPlluNB7HTjJqynxhvB2m8FM16LKv8DCCwQABoMNjM3NDIzMTgzODA1Igxnyi8TpGvqwznrWdUq3AMYwD6O673v1Z9112R59JXzMtAsp%2BahtoM6GQvQBgLynEgRnOkMgFtGRLFv2pWCFsTkHuX4hh1Z9ScS8yD8YFE1ve%2F1Fe3esMBMMmvbydwjwT7%2BuRVyi%2BDc3s3Ou1Iw3KLZzeX4m8QV2toEBhyFNQm8K1aUeBLprJeUnUIZYZjBW1U3I8MW6icn%2FRKYTFHfB1JXaSobCn%2BeJuguRj8aUcVsJIl1xD%2F3r4nKWDL41o5KEBnQBxhjbT9dSLYZgiDwoON8JgUOz6KcheDMHNwcBGu0tDZzEQr%2Fg31moRW%2FR1pv5MucMPdHVaEZmg2KS9c7e0jBCL8OJ5SaFgahs7fMJ4IiOv0H9VTTS1AnxilXBan%2BMLLZ%2FzEC%2BSTh5Pa1ex57Ck9RD1twKXrYcB%2FF4tI31%2FF7Uky6U6k4S1QTDvXCKyNHy4QdgXRX3K6517TSjUfSo741kFMEk%2BC1TkihDsma7hyZ2py7ThyMqVFwu5Ego%2FbaLddiYoxntrYh351Y134%2FCfnDiGCZ51sWda3NhZo%2BKWoQu5ek6bsrMZceHL8O%2F7fYe6Lct76CoCTUu%2F%2BAbxkfTNuk2fByJfFyqTTpA4qvXa7dAs800Rag3ZMlk9IAvmm%2F5L3LNItvHUXqO79v4zCu0sTGBjqkAa5Xx%2BAgsFj0NI7CR8nED9FYtKg%2Fv92MYDP9H%2BB2Xb61sMTFz94t0f9wVbm3nzzhmx%2Fibp9cksyOQL3EiaUWy0U51Aenmzl48JsvQS5nMFcF0WbPPmCcC%2F3kzi7zUUzElmc%2F5YkTbzicV54HNxs0RAdw6WxLuwd36qNKYav1QEdQP2%2FYhk07SA%2Fqbimd82XKXQ9oofBE6emPXnyO8cfjn3k6tj20&X-Amz-Signature=aba02a19a07da8ee84134bd2bfed32ea5f3fa54b5254ed0c15b50c5d6b66ebda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

