---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7JQT6VN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh%2FNN0VHopUgYkOzfddziOF8ofea75p1VWq9y2pEX9TQIhAJuOeGuXvmSU2ey5s%2B4acrZWkr71v2vqJmY8Yb5hqHHxKv8DCDEQABoMNjM3NDIzMTgzODA1IgyAGdQpYnpcnf8r4KIq3APg3MfAFpd1iBpMgy3%2BP14kJA6cL2ik%2Blf8b%2Fcvv2tmkWo4P2gjKkMNHb54NjiEgfdraRmPaSvF%2FYM%2B1upwklNMAq4riNjejw9YABuyxEK%2BpIFSyJKGWBcG0Kh9rYSNrq1Oe%2F7k9jBg3rBUdE%2F0kPrU5M8X38IrPORy4ceZlTZjlW4eENdLpaXKl3k8fVSETVkv7ZPE9ZpomrSmjZtLkccu5Ag5H2zbhSf21dyZEu1W6mOmeQ8eKezi%2F%2BkiCk89600iTqnKWyOhUobYHYAawFaU8HThUEqA8GfR2Rx9cfZoU5io5VuLQcsmmhYV5N9ROieca4d2j4Vzlf0fKESBHt7D2jfb6Wh%2FXv4qkIycTUMiu9plhNe4oKrpNEL8uWDyQ%2B7J%2B5s%2FDdMmSMmrViSMh0hTZFF0kcQSSZXzgiTMatngS93rEntEuVYMoe%2FkGsiLSWo7eT7ZvrSqyhQS4eD6tMn6cXW5bkJaeOSOcBNjyorptaXMCoGnDM77TYtD2IDUunmusXyYuCjFR0gmB0capRMm%2BMdLdTDoy9mjyypHBt99mKfxEd2Q2gasNq5C7lSSfcrU35Kn%2FWFnXRO%2BRSnAvmobmf1oTruGsHupEbpemvUSD3yDDRgHwpkcSOmJKzDo4cXGBjqkAaZCw0emJW1o6f8WZLJSe8FvOrGDQIETjSlgTHndAlDJQqJofbOURklenjYoqX607LCFu4nDDxCSnwZoT0Ob6soW2QPKvxj2j5gV2vOqIk8zSGJD9lEZMzeRUd0kCTMAQ7S1zZRdnvfU1Fn4nJserJ8dXu9EqlNxWf3ozQ%2F9Bvo5sBq4CMcl6BOB525CInkaH840lCcpXYZE9XPtX6zyOcNkOX7q&X-Amz-Signature=85d85521d45292f25f8b302026579361b6bcdd763d17a57aab57905bac12045e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

