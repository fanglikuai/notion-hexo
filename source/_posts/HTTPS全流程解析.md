---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSO6Y4NM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCFLsDMkxZ%2Fp%2B0qi%2FtmIr%2F5jO3bgSc5H%2Bs6lzKLnpUB6gIhALovxr4yupw2b%2BI%2Fk03QNi8XMDklIbiii8MUS4mHUrC1KogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyko7PcjKHoOBh7l1Uq3ANoyk1iRlnrmVb5vyGfC0XvRidzTdT1hawJfC79TjxsImEDbDcNIUG0vr%2FucRq6lQvMu1TF4aRS2VElqCxrZ2yh2neKWHx81C6P%2B0eysoykCof6yJO8Ia23ph3BtXL2eVZLOA6VtLWCr4CYuGbr49tx77QyCcqsHdAfJ72DEggVHUhFTVvvPFR0jRCKiuE3ACLDbOrmNq9tl2NMEsdcF2%2FJ%2Fh3pSAEHinNUCu82RE1V4bwju%2F7TE0wyM9VIGgUdyBi3Y95TNejxySzM5JWEaw8STbd5ytuGCXGVi7ubREq1dQ2PJZ8YWL%2FM2tDFxXfdW4uu3VvEGa8oTZOAIeeddaEgbEPqJd0%2Bjz5nYyaRCIH3vhUx3yPxmhzFW%2BkgsKnXoqAJ8AwNB9eJmY1qxV6ZFPCiw%2BAX3eeampUxZQDrqnqhr%2BKGcRZN7FNmecBb3T3e7bljchqW4gTJJ2oI9szv5khz%2Br31s0ku7Aroc4N8VAyt6DSkYpeD7fsdbxcFkliw%2BPQeSvPHUN69GsUPgw7kJecC8pcBKRjbZKJ7lun0O5sgPxUqPjl2Fm1KfNhynNFL%2FuySyNGzl6pGe79wVK9fNdVuY99l%2BUgxB95gmOLBB6K5j6CgKtGMGy0x%2B%2BkfQDDQ%2F%2FbHBjqkATghST%2FxJlKTLdc0%2FzBAnz5%2FT6DaJAMHq09%2FBxIinQAeK9xsDXhWZnTccKzRriG5heSwBStuBciWoEqbmtUiX%2B8gqOs%2BYi1grfcQ0LMW5VUEW74tRXR8VjplsQm2mEZt47qgLxcRT4a8D80sNteDyfepgIQve43w6Q6MUl5g3Jtf8JcRu0eO5E4eOeiSpefmwbWy9UomGKtpEbqcfoFQg8%2FCY36b&X-Amz-Signature=0d22b7dd32fcab65d867ff441d934875d25fa13093bb9859437fad8fb91ffa39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

