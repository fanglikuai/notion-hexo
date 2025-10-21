---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGXFZNEJ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T040100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQCfVtxRp%2FrbhVodCwFSLgL%2FBn%2FvaT20%2FwLk1CNWEQV%2F4QIhAMi8FKVjpKumiPE9SFknwPrIPFlXwv0Pb3ybW5D18GyNKogECPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxD8YXlSFDJ%2BwSK94gq3AMvgVOalU4qwtKL488EoZez3X%2F%2F2d5zqMn35ndOlHcMsJFothTr%2FZRHNJQZKCN4AdboMeq8VwA6yjB1GZKfPitdnLdUlIh1G4oquvmiKuVy4h%2Bg1DOkeyJUod2FshbtLUM3Zkzdrrvob7TWnWzFrN%2FNt05HRL%2B7eg1KCiCsDL37QUeLBmwiTWNHYR4GnU7x2aDERbviKp%2FUrSfnYvLhv39DSL3IjdL7Zop%2Fe1tBY6I2aowyzNTxwz9MwRLID%2BbGGI1fHketbsXfBb0T8d7XekSzUqgVHxwVgyxsXsGmCyd0HI%2FqhlgBMeXIY4075QKx1Sv%2Fru8SalALvyEB6cmrT5JY4ybYjP54clyxRHN0%2BwfSB1%2Fkz2yx0Hsaj7LS2h4j6ZrovE4GqhPVnmP9j0Y28%2Fl0Vo1qIRtBoVU3jSTClHtj%2BPaUTF5ShfXUvLmhtQSKsHUVXqSSmMEjVCWjKBjkfYU5ixIBO9gvCsaqfyGbaCwz15Ec6rEF8PwnVpt2U9CYk4%2Bpx3FeQZ0fHzHiYTo%2FA0DkRWdTM8nPomMrsUl8P14PC%2BSsP9KPak8nDo%2BvZMUykQ6rmmiLEIFj7PlEG1OyWNqL8OgDJn22FyZIfnTsYBmJQefuAveWaqqVWdZjcTCd6tvHBjqkAV725nr1GbxiMeWxl9kZSZ23xFU6ylBFF8X244P76yieKaTS%2B46qhPNOHOFjLQZ1zkgxWWDXu3yzSlNtkLYlDQdyGMsrsy7d0c5kvIqZIZ2H1u%2BAcff4tlFYMNZD4nb5yHNMJohGdn6y3DYMHWEahelcKFjsoUn8J25l6q1UGVNaVRPklrbz%2BvkJHF0dJzLi3O8JXFUFfJpsKUAigK3WofOzwg5z&X-Amz-Signature=80ddaadc2be31eef01f4703bdd158fc83ec567cea5c10126fe38a42d0a2a2bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

