---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXT7BMT4%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T000050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICpycJcN%2F0ye01Q2uZa0xGq%2FAO7dHm%2BX8Y4spTrPTbn%2BAiEAqY3RqSKP%2Bnm2Ui2oYf4oTWm99LJDxQxVna4gLyzNU2Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDMrxNiw0oAN6QNzu8yrcA9b0EiAR1F%2Bo4QEmQKULKRTJ75NkWiBjN017Y4x16oefDO8BBHSjs4YcWVCVDFG8PsSXHLW5f7%2Fd062MuAal56eRLDq%2F%2FglU4UITAKmaIcR2AqRWNsNcVazgQHMasY2QM5c82bd79LQr9tIVUSDIs5Kv4K6RgWawje41JZpH7Xsn3oxcf6ZVM2DL8viwBXT1BLUgbq1nlDqbOpWISfFyCgeZ%2F9Knr3nZW5isE6dT4wyYzp6SJGjjo1OwJEr4vA5yaAQzuPFXHFh887W1CNDjX3x2WO9WGQR3oVBL2Raj7XbeVkJEEIzdm%2BfqrzA1D9WG0dnaX0vcdVXdU22qseJMzLdVwmrvaXip7fzIWUucoKHY5psZy0wEq4pgAj6FcOxPRqE5k%2BH%2Fo5BfLvn3BXK1Yc3WqvGJqpHrJRKNGKPC7jDsT1QFzq5yMqL1JeRAychwK1gjc97qa8SZQBi7dYj1BuxiS3m2u%2FTpc7dl0T0wKAG%2B3ygUsywUIJXXSS2cmZOyjhSMK5jhevMcklbkZwJJzOEAsVr%2FLYDWsGRi6MNJ3A4jtvOWykOQxjzw9XZ%2Fng4kCBfQTo7Qb%2BDV09wdflzF%2BrudA9V2SKanno6PfdKzkHtj3cVYhGlTzHdUuJthMLXqsMcGOqUBcCD%2FyU36C9Kdv3iUqdyaRL1oXwG%2FHSt2DEu3h4%2FG2xsD%2Bov%2BEmxlnMoexL6uOy2gkTL5hYfROr11yOCTn21iqaUWeh1ZBtHOiU92NLSauckGua1Ix9E8G86HwFAR1UBxTDTpDZ8BQDpUswVsC9UnUBL8a9FNoNku7L9LCfWBzTkpkQbAtMi7uUzs5ymKmUReGuV0lzv8QAR5yD9fadnAuQwF%2FJSF&X-Amz-Signature=e7ef957312b65c006b9306d0449cae2abd8faa66ba7292a7f35a367a3a8f46c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

