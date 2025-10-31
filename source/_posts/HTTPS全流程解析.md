---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3CZYFVH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDhuynwHjY7bQ6GFwwy333G8zSTMzZrb%2FW%2FwUZVEzwsngIgcU4WTo7TooaZ%2FlJAKfD0WwHFZ%2BUq9Clx37R5b8A7PCEqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGSmHzM7IyKdcoCKsSrcA88s6jkSJDh%2FtTj92CVqO5p7untDopRV2s34IsWZWg7TW3SIIfx8jPuyFsI0%2BBeNAAm29v0l%2BUebr6TKMXmTfhG8tecrGMXT3Tq%2BUojhz1U1ybgys4HIfMq1uvpJujdeE62CYok3XAQTxD8UHImYLTiGRqCjDyEGdkO03oSyYqqKC3Ew0KCx5zKo%2BNrQEQaoaqx5B6c4kAdY5vH%2FOzXK2A%2B0HWve7Pmgm4KoWhGUSQ38%2Bl%2BsaNc7RR7%2BQjkoPN5TYZgMoZuiPTpp3A6H0DBZqc%2F4RNozESqi0mgjFaXQDt5Eda6wztc9Rb3kcqhsambaArJBxivKAUKCLmdUD0AHCgVKGA0Zd7E9sA9sBntzdXJoNwCzMydgKdDBBHeLktRiAWPTakS%2BuVRbrFoWMCPyJDFW8MU0s%2FhhWYkRqzqpa9DhHvbVltUFzXJ2%2BcJKn92QWW%2BYBpHFebORg8sSOkRQ6KMwnRK4CE8rcMhS5veXjGASMoVwmqSrmuYMfSEqeSHi%2BM6xfbsGALwd6AS1Yc66YZAgpoVNDZItELiYUcaGaB0U8mIO0%2BUdw0Tquil5ruBKWG%2BB003PDSqcGFneX%2B2ByMrBKY%2BeYqzFVIE2Er7iU%2FmC9YrdxLzXAodXLJQfMOqLkcgGOqUB5dCJZFWmBXApG0t%2B%2FmrW1EH%2B%2FnflY33CjSiv7UBq9%2Bf72em1WZjdXgMKlDtdQKP9t2w1nLdTLfvQO1KiSFxO%2BiQ7ZHXP6Wnp9f4IzO7ANjSM6ocniEKbJWV6GPooL0cv6hZmxvLc0qlXFdlbi7uXOw2fgg69RL7fB3W52UQZHm914C5x8ufweXWA7pPqFW%2FjhI8o7I31nkaopacVs5Ah%2Bjp%2FEjis&X-Amz-Signature=b9265bdb31fbc49b3bab87be3aafa69be7b946a138326480213f3e8b7bf16513&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

