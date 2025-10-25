---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZP5RGLCK%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZ1bwyAFfgAYm1mFNXhtHGC7ZPIr3XOZFip870f7ZYDAiEAnaTJyt%2B9q60Lj458ZqiGKsgvuz%2B6ZvTe%2FZUFi1jOE2Mq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDK8E6Wt0mSJc2F0V0SrcAzqca5gyOCVccC2k8ga3heMse5Zue6Z1xKFnZ5As6xvTiO9xoYHKVQN0JWkTCxepZ%2FLvLeld%2Ff9w%2BcuCj%2FSlIKsZZOP3nA78bmjsZ7EO4BFUHZmgnYgsdiGTdohNksP6mmZMVQtEdwYX2tjtNOGF%2BjCpdHeAyleoTgp9t13iEoFll0OQNTW9vCN2JK2nVSgiB3P4nw1mz6I1OkEnXD3SEsKwX7yJ%2B8gosiUvDaoLnre8TlHKePMq9wUafEVbdUbf0zO7Bp8YFeDGPx5ne5Ch1WVhTsIE5qpFYd6nfG1D21w%2BEv%2Fm8tLf67fAYl3nwtHwpHH69ATc7%2B7CJ407tkYnMap1ztiMIUvLY%2BTbg1l21CUHG4XA4wG4Amf0RA9TJgWg8Yqk3H5cSMLGUfJsqglpjcprNVnNX5fv6muauMgeoLD7oPDVsRTwYRgGJYXPCrJWYx2nw%2BBJ2BnTUlFRbC5zNsd788lme0E2Nvr72HAJp5GWgXVIFKA3l4%2BZ0ufjODgDaYW0xTfAYBDegxLxu8x1%2BjDhUe7kOTpiokKMgdl2VHFPBCrdGkrpqnwVXv%2Bd0heXD1na1SmQdUk1Xf0xABT0utL%2B157pv7DpNazx79Ei5zSsZP%2BABoJgKNK9jZ6cMKz488cGOqUB4McnxWis3NegkNNn2K2Px9hIK7qmOsTTwkPhJ89jA9cKDCWtnd%2FFuIKSXUrwlFLEra%2B3LYMC6Z6nVBiMgjkZ9jdH6U5RxhQ2tC3miLGT8kdVCicFfp9w4hXGzNk0xpdqK6x3HLX06j9nD8phIDOy3nDDxeEOUaO%2Fhnr6z2MFfrlpyF0Ll%2FGTm05Fza%2B0dl10Kz7JXOcwMLe7Lv%2Fup2d%2Byfvrp4wk&X-Amz-Signature=ad17a065ebd39107070be6ecfea0f75081b3c3a0f458485ea3d56325f80c7af0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

