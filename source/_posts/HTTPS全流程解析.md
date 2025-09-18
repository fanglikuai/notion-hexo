---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKUMN3XZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIESVOizWbIA0%2Bf0idUb8rKJSfL%2FMevK7PNZXRKMZqfVoAiEApO3VZgNdNV1e9%2BUlftj8z7qmefPw6m5Z9KUZgv9CE%2FIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH87K1nPQt3%2F%2BfcS6ircA%2BuuRtUSzAMb5mro%2B0K3zCu2r8zmflzYPSvHHEv3jIGljJVSbMTdBJn71CQSigLWt9ucKLFoprnfGmgc0j9hx0V%2BvE3HmoShMs%2BHgs5YSV76gmAB1FnQ4G%2Bni1apt3WlYrWQwLcPrITPKk7dMkU2BMhth3dW2nsKQksJTDt3KiAtj0xeUG6Rpd9dTcvlryeVuonSeq%2BIvQ7OAtaDss0gPg3XG50DXWVOi2UXmMQ3H9MZCrh7i0h0IW8fpzzu7%2FfBwH%2FfGQMQqTaM%2FidshezQpDyYUSkW2cCzVm4idmsSwXpktvSoaQWfSCF%2B6EWKbuqjn%2BB0rQ933uwhjOtAL8QsccN4R3DnOMbMyFwFJ166BBeKVs8IL0NDgz2xgqtnBRARTNYV5W3AdyXPt4XpiK%2Fglz2%2FpkO%2BMJ5JS8iA45PTw0P2OeL1NHwS41mo%2BsbZ1Y6w%2FtXrR8ol%2BixxM%2FiKehoV%2FTS7TbU6Wqi4%2BXGX0mYCepU%2FeM0ZRQVFXoQstZfrtdo5KGjJYRDr1RfWsjnp0g86pEhsAXK3dWcko8LjDh2MnpN6CU66TOUVo2TjXvJKx%2FMitJWzMneEcu%2F4tNQQyyD%2BkZvprKv7zOuTj1C2TF4wdsZdB4ELOZjyyGTiefbEMJTZr8YGOqUB6SQEV2FB6n8B0E8TsVBth9uKV3ejnrfdU4DV0Sp5wZUfDYQRkXDIFomIDKQD6TM4NWM1Y4q%2B3mkYBENQR9OqfgCWouLxlcb4eUppkr2262FNlWtbjBa29d%2B7aU8O2xTGttmEI6Gn21dSEkuK8J0%2Fze49jajq%2FkjNMfku%2BK01KVgl6Jc10fGiBjohkmnfHFVzOQOYoAvUBDXXfnPPVIL5lWrJcss1&X-Amz-Signature=49d425e1facc69be5bce5b658e467cf07d28e8134a51e370ab5167749d11d939&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

