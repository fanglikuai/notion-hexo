---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NSYQ2EI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIG5jncR42BtGlAmUKbnpMu0RfdOU4CFR0iHPSdwDMg9EAiEA39jAA6qN61j8MXyf7Hdg%2Bm%2FOGNXLpGodt2%2Fh%2BbBZEx8q%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDO6uGh7cJ7ZFuxXadircA0lU88mgZUDs7TpTJ4n3UwhyTh3%2BQElNDnuabModbGEnsZv%2Foom5OWxv52AM%2BPjolsbdvSjjLwb2SjAKAzkdChG7rHFvM3iM2bu4VppOsToqGI8Palwzej5FSu10WJb0Hb1Qwqu8Gyyv3bE94p%2FnwjmU2Lk4ek8ixyMnEevlBQCzA%2Fkm3NuQwJug3T%2FMafzKJnr8jYrgZsH2JRRYRCyWuFlNaYDO5shDSIV76Z8Nh3jHeDPTYt4Jn36tp0jYnI7NSbMOb4xc0Mk%2FrU7SDFsVQVhk4eAFSHArAMhzEpjvoVggap62tk1sMQ5OtGL6xPuovdXSoOcg15mihcuTMjDpIR6CejRdixCpA4R1IJcN7KR91BGnyGZrVXZFZA2jD1PFrM4XuDeoHivEELx%2FOuIubDXa7LZVY01AqDeJnag7w1VDggKuMVfPrda7myU03aeq7xWp0vTSP7FuL0XCWkm4nQKKM3h0KOrJ5kOrcqTMirm2pqR6cPugw6VMc7ZLkbQvnpZuSirPVWRJi0djbhkikBCkxB1xd4sWbCtQZykJlYo88evhmHhhiAO4OmEleXVrezxZ25aiet%2BDeiGlsuODBJYF9c2KnLDo7exkrKLE79EeTu0zd1FKkIKkt4dXMPLAhckGOqUBEL2EPe9d4UyhKoAlMsRorQVQNYgqtTgPZNeOAmC2IitD34aGJejJkldakkv6n1Sl5XJXwQ%2BzyXZCbcgsl2tSNSnigsw%2BlSXSTuPgzYyHNpiuJ9iVtkH%2FziF3zT%2BcH4Acc%2Fg9K2Ejan6vBDumzzQ9z0v3CNJU5O9z7Y8NeLyRZDq4p7KU5QX71l3mc7tIVSKTH59gcv1pEcdwO0OkYKaOG4gtTwDO&X-Amz-Signature=8c1a1c211e4641b1869ea344103a680d54029956b9189e21bdaef5fc531ac0d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

