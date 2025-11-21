---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVHR3CCP%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIDwk1s9FGPNnVncnjy2V6UVhi6uhOIuS8yOF7aj0ejCWAiBWBNfwI6SmrzWflgHV%2BW8bs9dAahY6%2FJva9wTKw6eVUyr%2FAwgKEAAaDDYzNzQyMzE4MzgwNSIMDu2H2PdlPN7sNzu3KtwDihe5trfvQhiHFA3G5ZD2kB%2FGY1cqs3MdLNf%2FF7SI9Sip4Fcr%2FwspcdWWMHvXtc5ElT2b6706JAy5lS1GZ7MzFH6EDb%2F3%2BDRStjVh7Vc%2B54hlUBzvcuxIyn%2BbJGPafB7CXlo0bqO07bnUsmfQhH9ns866HqyTRuj6rurYWdTP7o4BS06xaMxKwDTqUOpPePFEzOrJJZr42TbNNDva2%2BGnwgVcUvit6N10%2BzE2FQxTz7G7u48dFUt2MYfotmjTtcIU2XDNoWSTS5ABgmHeflUUbMP4eeBltDE0d%2F4eINz9%2BBJR2HV1ywYkP%2B8HpVGGCXEf%2FdcWPY1mTul4DalfD7TRvPSSrKMqfuFw9WxiTYikiuGlhPW5eq7MY%2BmoaGYUpNnEtrykxHUZA4aZHk6WAmcLhLSdoyS8tAOQtRYYB1rGAnKPZacBkYcaeizcs1hLz0ruuqqJkZcWvj5JW99NXBa1%2BsE%2FmTupcCJVgjnWy%2FsWnzfRmFygEq%2Fw8kcVxmNWULLVjfp7FGQyxqxhgHz7l3PfoaAoL38k87lIsiaukT8rh56xy%2BW42R8NnGx1SSUXd88kYta%2BeAFA5c3FAVNN3VwQHy%2FVpvLlzibavKLS2eE8vpMWb9PqvptuQMBxHkIw4dqAyQY6pgFv8UUnKPN8gsr50VW9lEHSR4ppei04FOuHz5nxc9M6cNTBNsKg9SudlCxJTKiAgL2M5C9aCPVQTA7Jxyjwa1%2BwGl4WxoDk73wpJ3whQ8x9p8SCeETIQvzjkMDmM69gt8EpPSFYWJcB0xySl4GKIwLTFbj6MIAKQ1HbWQGbQsDcT%2BJ70fi19tS8C8dfAesvnhc3qovQutlbIC4cYECBukknVFdUd8h%2B&X-Amz-Signature=95132b59ce5c5d93e2ea1e4770935ef4988922802085c7785ea67408dc23e205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

