---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNFWOKLY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB0c4dCKuXANOCyVjhQtb7S9VqNLW8o4jgehwslMQMZ6AiB0bWLM5i0sI0vlR0XG6%2Ft6w1miFGq6bUqRUANJqf%2FCfiqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMvLgCQE%2BhiZCztsUVKtwDeoeBUxsKmQfoeDD%2BU%2FJY8h7lexoSUHvJ2ixH4RzvQEj4jbXnpfpW4PxuVhiW7dvYNgV8nKV1UDd7LFNp9%2Fe6WmLJUvObW7%2F7CEwyKKk2wuWjpuNezl5ZlO6QZB0ZN9FE2Oawfy34HZNnS7MfV3BCRBF3Io%2BWfXz8OOczXo1sylUhAMJmVzK%2FR3PV4964vo0mHBdtb7h1zK1Wm6ZSgyWTQbxEfBocrJH%2BlCHgEc6Fu6gfpvz31%2F1jFCq4FQ2mDUWCXKcS%2F%2FiDpT4mgSAZaDn%2BI77t0HVXCTPFYIctgpsOyCdBy8OlE34IyJSWPVtivbOnNgnWjbtY603tdaIsI%2FFLE%2BFZHzm9isEYP%2F1cQMWrc4ymysl%2BWcPQ0IN6mOKDaGe%2F3J%2F%2FAzY%2Fm6QDeV1Cx4aL0Y0wGHpw0db%2Bk4%2FtNDWK52cJghzXodQPMuPgzlNA85n5AqHWIluFUAJmfo2gfBV1JLsTW9dM2qQUrtk1W%2F2dYtldGnX4LFsPlw9tPtMZ98DDLlM35fw1YEPhVDL1uDV42nj1gYkhuOA945kHqF04UyLB4TUKvkmwKHs2mHvSIkX4mTADDPivxn0xjhDB%2B2rXz5wNQTY%2BvxvQc4hpCWEEGwpbEQtM3FQgarMUWvwwnLyayQY6pgGnARoJv1gCf2lBoMyDFMtTj76nPg46ey6uLUifs66%2BK437xiVjQb7uScnLwbm68CG5pB2UoyWkUvEby%2BZCq%2BBNXlpeVm8Iq5QCoF0gCHwkgfvpHxM%2BebLDsAH5ovTn4UtLyXavqqhdUXRVL5K2o92fmK3wGe375pt914SYDuJjp6YNafPaS5tEfi5jgGxovxd7RIgV3r9%2FjHl5ZJ4JenfD9bbX%2F1tb&X-Amz-Signature=2d88b055bd7d3c8b129785ea61cc6811fb0c4f3a2bcfe83b985e3b39c7329bce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

