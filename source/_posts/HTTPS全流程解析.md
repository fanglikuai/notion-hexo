---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UALPTGLH%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T170123Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWZVekGJlG2g8PYKsUnMaO7waf0gtQPAM0dOLyfdjFLAIgJgPziuyqv%2Fkyeq8Mxz0cDwM3BV1yKj%2ByvxVylhM59f0q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDFq19cXEd96XKMzPvyrcA%2F2%2FSY0Dc%2BzFjnWQsij%2FSvb5vW3oTn9NZuvTGYnh5r3bIYh7VO9qJ6rlvuq1hv5byjN%2FOqef9f3%2FL1ZKELZOD6HsEx03Iu0mlowF5glCt7IbasAyTLHJmcq5q5XK8mFH7GOiFuDKQIVlM2FWa4TfwLBq6BwhgcOjyf0MUxtqiUs4XiE4lMSbUK%2BjPgt4mJPfb3eUN%2BmpeNfY2qROJRln5QcJTHzRrOQC%2BbTvhfI8TBeWWPza%2FtTpSpmbY2ACRzWZOnSr%2B%2F%2Bdh%2FYqBWmzrLYyc1%2BXg6zwq64zGRCSj%2B%2BQghCbCAEPoSLsSkfXqQ6cFp5bDW5lcwDsQ%2BheYN1dm19WTXmxzuQcgdKSqK%2F1Si43A%2BbaGuFyWZ1g0m26F2QbPBSDYSC0drtNLM2QaUmWq1tjcIRraF6TJkxYSD46BASoCwQeGxc7yU6eru2zfxNV5yigBj4N7JbRNfhbJKHiS39XOxkSt6lal5P64N%2BkT7z64uHmc4gxts4exm5TSjVEt972ZM%2FsH0VpYhEAGzN2EZtJvWRdkNCKp9XbZOKkyvjgGLKW9NzCPUhKlgFQpap4Z1JJkW0QkabhINHFFQqYZeIvAwxGhz5UF59gtjgJDoTo0mbJFpLZoOu9xPP8fOV0MNWV2MgGOqUBTtsfN%2BQDy9d3gvCNPfANaubd1FsKWeHAorN%2B9rPRL2FILD0y6LdrcksN4HsDCZuCrsG7%2FO1%2FXsBdyQhklBnjUkqZkMCpxlOuxnA2NiDjPrEkRLytvGsZ1VJitDLG3xfdWFGBzRny6fUh7XIpHVihrQM%2BpBSAWE0mKrK02ONg0DEOaAMoqT%2FMtC%2B%2F8uo4L2fqkFCOMzgiSwRM3i4EDFtbYrjFwWxr&X-Amz-Signature=5fdca6e17352cbed836baa080ed701c26043c4acf2248c7bdd8db37546d0b877&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

