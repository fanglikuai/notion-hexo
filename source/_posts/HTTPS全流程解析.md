---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USCVHRXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqNgFDcoXQYATk7HlzSsUmUjU4DQXgHwQvtpANY3j9dAiBwAmoFfFxBS8gnskjtkeSm5yHG5RZqRhJqOxr2CWXLSyr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIM28n%2BC7j8%2BeVj7BL%2BKtwDi6waA%2FEeVLOEHvzjKXthIkNjWqzuEmNQiJXH%2FPc%2BP5FrluHwvrNBV4Qy4n8qggH1bN1iXRVgjvmr%2FIAkoMXo2A9b1FOVJQZ13T1DP509559j3XKJM%2ByQGiml02TJjihDWBOOn69XieJv%2FtoMtFY0LDRLC92hV0XODna2ZkH1a7wpcaW4h0RRR6KsybnREIg1yu%2BFu%2FGPsbJieBR77TeiQr%2BCi9U%2BCCxkEqawk5GPetlF2aQQIAaTO0Y6G9eDkKzh96RfYtwJp7S%2F%2FCVVmbJmZGqdnU%2FqZPn4GyQ%2BLGRnrHH6Cb8hQa4yaKxAJ%2BMD%2Ffm4AAOlVsf19JkcjyUrLUEh%2Bh37rU3xJqs0meCFSPCJPs6DX8dF14BmSgexIhu9fcd5Gs19kFYWFzSBPbf1Bw8w%2B4MMlNyRHm1ujMw6oiwtnUrmCIKvtRYzOVRkzp6yX6Dihb37Xfl1gEFfF0hAcTBBXrFiDQQqO9C%2BGH8916sOhnCM6HLA%2BwrzRbl9K7sFe%2BJPAskI1Phcw%2BMVQ9xvItm4LkhqOLy1ELTF%2Bu4uVqGcgR4W0q4BpyBZs%2B3B5nF8jaxx6RVHfW8evcSXl0vzj6yzXXkFYqa4dm7eUtfSwXwwhjRhKOoVIl%2Bwu1G8XYww36HXyAY6pgF43WJ81dPhdm9AyRO5YTlp26rXBXNWJ1DBC7P9W9jByEWv1Gdo8vxI2K0rsAQg0clweUj6qUBYVoE8qQzq5rJr2iMRUtiGarkP%2BhTYckaiL8ko6WUx%2B4UO5GFaKJsvYvb1OXv6WtdKjVZ7O7TWsCfAVbxP2ECSQOi%2Bq2B8GUiXNSr2C0BjVMu0bEIGw67tO7RpioQJKQi5pJjomW8ZTOzAdrlWOhBI&X-Amz-Signature=1c7008bc61c6358b9569a88f9c7d2393f2d45fe6ae78a59b60f98de4278bd3c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

