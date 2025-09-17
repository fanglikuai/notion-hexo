---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUQNRA6A%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIHsBZI%2FdWcLVmm4sDbrASprF03DDJj3gv%2FpY7Jd16rxjAiAk57qt3nlBspL9GkGgzZnkiuD2ZSAqn3l03E%2BEb9Ib7SqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMb7tnqysV4Lar%2BOKqKtwDLtjY5ufDAMFD6PVDLG6pZAIc%2B2UqC4Rmo9%2Fu9ayZDl22bfz4VdBo6KfX2BqxqzSH8x59XhIH7GNWnsO4VV%2FTLpbB2whKMxdmdZB3UHh3g3oydcEw0EDE3o8JO4vkKQkOcrP4qIGGqgXA79xL2204zHqWtX16aNlOY8iADWg94MetcCdAd33VRCYg1AZDY7oHy0fbik7Sws96mIUegQAtlpPvZ5zKKKuud2KlUUZamJMAQ5vtax5ZkpvEhj5IqJHdU8ByuOtDbCT%2BhkUH2wCytarQA%2FohWxhwQ0EVlMb3kJ4dBLo5zVvtQwjBHX8hPB0ovHow9Wg4gpWnIs40ZXtozQtyuNAWaIcIWyzLebbdaoNu9LWywXh73CL974wXf1vGDaQ7EtCPzTZsWZJmdU%2BOGw7Vv0kq%2BQJXJJ1DSVK%2F5JlazZwhaJ9yrfpLIW70N6i7IywrHvbdPyVWGF01MljY4G8L8fnrSVrRujZeHTz9Io76FtQFVFJeGgtt5Ga4ua%2F5pf5vNRPv57Qtlo8ypw0Jh57rkxQW9W7MNxB393qhmvh3h0XQ2kZY75EwBEYgedo4dN1OsX46odgy7O6sAVncVoUY0Plo0CjC9RWcITK6fwg8BnwHnUh1jvCONAIw2empxgY6pgEH3bwGL81i6n9233sC4NxXp2eHmdplnBp5mrGYWxs7wEkxiZURfVUhaceDc4cpY0HcHzmH024kUjFycVadAQ7tLdWblMqxtiat2b1hiTEUoHu19Jd2vvDgc00xJeiDrN4%2ByheeT8rODZ0sGWv1UkaEkqYelCvlcnFmA6dWfDtLI2vnoC3ROh0QaE4N5Ah%2BQ4sO00mKgv3QcCPotwuQ3uSEKYTTaR9E&X-Amz-Signature=4468065187eb81131cdfbc216728cca4c23189eb43654acbd83203a6f24c0370&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

