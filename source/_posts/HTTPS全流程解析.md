---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LTF74DR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIHDv3Z7CapZK0%2BYKqROdOvrfztqTNGTNBIVBE11%2BXvQzAiAwwleujgFvn43bcEPiWl7gtgJ%2BONsvy5TX33Jb%2F6VUByqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcxDttIf5REqIe6MAKtwDWd2Mb7TJj%2F3osAlxZSLRDQI%2BJO4RK4H1qVqpR36rIDwMrCspX0pn%2FhmvvHtUa3CFQxibj%2FOOsotaHXR291bWA5RDncWkJp%2B4Ahm81rL2OXT2wFa8cjbUy5Zpa94PNkByv4KEj9UBE66cejIhRwla28LgM71BtLSFnKLN6JUAVByQd2AFCJntzcUv37O%2F0Zudpm4YCJ5pAtpkN0Auts81NrMS%2Bbxpek%2Bul1rwRPpm1BV7Xp8A42BI3iHm%2BWRE2XhE8h56CAh1TzEc3f64YsnVOlwyqqbzvPYSRXBSJK7mfx1%2FKQ44x8YqFI7c0ILIjwrnQXp37dMyMploPG3XKWWULR%2Fe0wvnrdLxzREdiShpwIPXpCfFhPERIz%2BL6v7KSorLE3t8baqVgIuZ4xTnIBiVp7zVuhBDiQqsrDkHmrbQDrr7nOQ8HK0PEXTUSOjJ0FCkeBPtw4kHehXCRqkON2GgFUb%2FzmKtMtd%2B%2BNRHeliuTolNwgwpifGJJEsaS6REGuAcRwBUGg6JaKgGyMIliKKzkiMvQDD4WStNEPSVWqt6jL5fPFZQiy8lzPvYN3%2B%2BMnVwDXbMiSA87y%2FrPs0j7XGOtP7EDPCaOdqoog2QSVhdDVzQeHLcNcDrtVO8JogwqpuUxwY6pgF5u8hXLeau2liy4clenWLf%2B05YDksHqz6iCW6az8tqtJYlGNssutf9RepT%2Fz4W%2BmZxzLUm26IROdD%2BH1wYSMCtcM8hXS5aSVXjYjbQLok3HXZhtwdZOsU8Y9AVlGN0ygS3zKumEOUNE%2BrvWWMV%2FKMrXM6CVKCYCmU7hkg7E7ysg720fgu66f%2FLhjJRb60VD0iWheBIWiVknJrRK0grGZwiQ2HELxlZ&X-Amz-Signature=8e7659cfc6fc5556ca4b714b3440f4881d36b676732f83ab51a7546ef961c5b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

