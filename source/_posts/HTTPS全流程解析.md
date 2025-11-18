---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBDADZVV%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE2PVEvxDPlpMMLc47NKjlYVpHMaiZeAexLsrht3rykJAiBB%2FtrDxnO6LhSFE97UClTSHEqgAdZj3MP%2BpFL5L6T6qCqIBAjC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEE1nBQE7hx8mfPyqKtwDHxPCnEGUt1XbH%2FVxwiG7CWQepxDYqWw7VZZzvvEDkv80xL7m2U5PJRzV%2F09DrsPjZidJUWLRo00SAuRv8HFIgfZu%2FZtGeIofIwMtuK5AtA5ugVn%2BvQ7BK60O8VS%2FN4JYN8jfQTQIojuI0CazHlivdhGj2umBcuKhO%2FwkU0kIqSPN7CnanVU8qp6SXT0UUEjX4Ckv7EeScwktRxu4AWFKf6xWvvHXgjsv7A1Rz9mrEQ%2BqG0GMTY7fP783jNq45aafqy%2FrXIQRtAVyqVHLhIbwD3L5WsBJM9xWtOYqMj%2Foa%2BFK0kL4Y%2BESc6XI08E9QF5bMWZpwyOChayuQz3IuxUgUYUJMJCgKYWEEItUal7LJfc6a95U%2BGtuTuW1ApUl4F3pclDeELxHskLqw%2BafcSUYmp10omzRCcPkGBPLZt26noTCuxKnghgF1u%2FrDNK5p%2BcH1lczaTcTUtTprurnOlYr2%2BUafhD%2BHnS5fFhidjdt%2FCX%2BA%2FxSw7X5Hs0wlPsp%2Fgd14l%2B9VeRR%2Ba0BLCHMTa6ug4u%2BOF2XiU9Gigvxr%2FRvAck7srASoSmrb7u1hDEb2nOlT250iP4V1qXZXedLPDk5ECfpQeQeHrwK2CUIUqcVOuImm24WuDwfU1NUNhYw0eXwyAY6pgGEoaV8aWs0NQzp8uwDYZWpXuD%2Ba%2BzRKoW8LoCLcjUeIoNLwrctOq%2B1ZEoqePnEn8gnENWaFEsdMQ1AE1OUeXTJxI6G%2FEqgRU9QavpXDRMM%2FPr%2Bi7CJPs1W6hxuzzj37r7acWU8eZWKZ8xHJcQaIcuV2ozg%2Fz54eei4CJBM9KU2BVmjeHzGmLl%2FSHx4%2Ff5yTgZ5iSVV95el%2BOCQgR7C6eQluXRyV%2Fbo&X-Amz-Signature=12a4f54da6ebafc60c16d4e47fe7694fbb064fac6acc3488a789668d7ec5ba7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

