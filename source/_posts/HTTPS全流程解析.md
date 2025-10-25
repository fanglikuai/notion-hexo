---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466273ZX6ZX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBroSX15VoP2IQLTOL6PzziDO3HQnEwSlbE2lfkdjVpJAiEApUeFZR%2BVuFuSuObxkArreILjWVQNQo4R%2FenH9jSLTWcq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDE0lTOZ5g5ho5Fwd9yrcA%2FnykHaB6u%2F3%2B%2Bcs3Fyt8yeazukZT8oIU2dmrxJ9Yf3ByuTMu8YGmZV%2BxJgcWIOKFtZ07eV%2BOhspSLdLQ4Bzs8AG7K4VT4kxBYgasF%2Bf%2FLLvKmuoLj2QiNWJBBcoRBIzfr3q4sGQDa3ZeMrNg3iIdlPZuOOlEw7XM%2FRR8YKqcjVJKtkAxBhB46i3QVL8DcLGbIbbMZNmOu6dYgh72MpOl8OY44WCfgmRIVmO1vm%2FgSlNfgthDuJbFWNGU%2B95xg1C5YxxkEEGfN2wbq0RdLiiN6oJZXthemInOu8c1XNbLmXOiilvp9KtnyW12RuU6kIL24PeYEgsiqs0vdnOSEYdtRNXJ%2FRXMRI9%2BF7StPCt4gwVpVZBNPxTcUfckgc1PZrrM6j9dFXS%2BKrQqSPltf8s0%2BFIrUOSdwOg1ncmt%2B5oc9mkI%2Bj577k7%2FTrqP%2BqGF7ENrnRsdTWb0QSedyPxan6TgC5C4vivgEQCTHUSAn0ckfTn5slYtij%2BWG93%2Fpcq1kjxobM%2B08foCoEPSM6zPeyua4Evo3NxFrgMKz96ehRzfcyBx%2FdSGBj%2FQd%2BU30wVzKBcahFLYS6ScDUSxANJahWXmKGUmbt11kfWEp4Vmtg%2B99WaV%2BmXDIA1Q%2FwGvPEdMLmz8McGOqUBu6KCaAjAR%2FCP7ASnU8A0yHI6K2A18YoX5gFZ5Og06KuguSdSu1LmmxKsZSj6oikyBhEqEFfVYIQg0eDqegPTE5q%2BG%2Bbb2YqTWrL7OtuelG4%2BwFrgudWzmsZvnAHI8q7PRpDgRondNlAHpnut766t0TQoC%2FoYulttPaFpH0ut7h%2FXhYZSQSyX507KWtv%2BBESBo9Nnj33DIvPLTOsfnfQsY2gkcKG2&X-Amz-Signature=e4f4a092833f3ccebde6efdb3fcfd3cf5edd8ce3600f65515f61949b441c6600&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

