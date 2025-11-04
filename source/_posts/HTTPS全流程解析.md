---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KUFMSMM%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4HaM2J3hV%2BO2nLz4UAa7ULqi1kOJRBwKPGcPwO8HnIAIhAN3VxvG6Lzh%2BC51Mxvrm6I%2FMSncKPGonKaw3PvnIB%2BsrKv8DCG4QABoMNjM3NDIzMTgzODA1Igwn2EHTaXk3RFGi4eEq3AMaZXwZGECLuM5UHA8%2FLvqPG6mHPPh5FZqLBAbqREgw%2Ff2WaThSaMqnn9TO2ojEnsN7qd5RTBfePJFq%2FcPtOiAlfwA5wweY3I%2FGtN7Iu2V%2FcbiKYWliab4oA0ZxipRoSI6fS%2Bfj%2BuDicxiNvoo6nAniLIhggxxPrTpm85NXPSUxQnchkh8Dqh7f%2B2UZrr5HVyPkbBPu6rbA0n3%2Ba04bRZC9Oc6bbDMqMHJ57evzHyLlIpaJmrIifQtfcd8G%2FhciURua0wGD0ZWCQ%2BOq68oCwxk16ym0U0bYGFzcsvrbZqxer1PK%2FJzOJmvqRiK5XdtNgHhg8XTeVp294I6JhRoaGTHS9UkV4hW4jkX4q4XNhzJwqfI4uJsvEaAK3dSKpTOIoLbcqAwoOGLNWRXdxASEni3LZDTBxkc1qxGLDfidKlIzT158ODOh4YR9Y4DfSXnsQqdOcWqbjN1h1iN0jm0eA4EAAxGIfUVEo0r736d%2BzB6xv7Yh0E0pX8jf%2FtthcDlnwbjr0yeWlTT%2B8UZJCiNT7FOAyB5Pax2cJBhmYWhcilkA2M7hXTj%2BVrcYDgohhdYzIsAqGnZMdHpY14N8RcXTENjaNJKLKvLh%2F52UTXl4mgaOOyeJqDx01CBbz7kzGDCeiKbIBjqkAcBpiJizr0WJ%2FCY5Wsx3HcRpMcQ28NqxTPUTaO1O38N5I96S8pSohf23ksWRaVg6sizMAXjaFKRsJlYSduxZcQ72VyGgU9Ij8E7nzOhGcTnUHox1947r0HLj3Sya9oev0rhc5K5EGGIgOJvxU8vP837AEM0aGPCaiodGRq0rN5blDdSNk3mZZGqfG%2FmDu9YKts2pIIJ%2Byn6cNsyQ2rNphVgPm79k&X-Amz-Signature=dad022d3c25073561697b11bd80feb52ecab58ddcf077174dde0fe64f5fc2a61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

