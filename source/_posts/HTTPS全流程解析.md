---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDWQBZK4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJGMEQCIGsxXTEnxF4atQMxy4MiBnPBwk9EYaYgxFGovmfa%2Ft37AiB2%2FoaKvM4cq4%2B4xWqwFtQpnQk%2BmMzIMLTKe1FJCjg5bCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyUkThqFB1wlg6HOrKtwD12H%2BM2rq9GGQqk4gvBUzhs1gPH0K7RPd4MIgCoDVglUsJ%2Bwa0q8AWawbcU%2B0B7nzsqfLIaS49hHrHMFOcRzac0ofmhxej4jFRkekacKSt5u2wUdnR%2FMgORBpbkju%2F8hpF22GtZVX6ikTZhbxAVGaUvosOAdLicbA609hB89yESfR53cWVgN1fSSXuMf3bbeocw7XCWFrcq%2FXnzYMGxgHhP7T%2FxjZSe5uhWYbU8qXMJr3XjLPobtOprogVy94yTm2RR8qcrhoEGH1xm9i3Vd5qm22d7uuDuN3hsVNCAwpd7w0YBfIaE9NDlFFm7drBq9QlvDRLoA%2FsDGQchz66pML4jYxf4oQTl08hqFdfT0AJuLwpfzxcVDNegt%2F5DJpbpj03thn9uhK7xGB3nrz8gNAqQe%2FI0ffiaLNI%2BXoONjpIraPJR9qLDG4wE%2Bv9GXCULJyDc7MUdHG0JaUfvQgQenkZV%2Bn4viKVhyAfMrso1m7zJYY9y6VuRs3r9BgBcRmFRHr4rjNel7TXbOna3zSZwh5mRQMPrB7rFB4NpG8k0Pcvs9%2F6%2BCr%2BWQGvmm49jW4xBEtBNrFhLl6O0G%2B%2FyoxLoxkrYX6QAlWeDa6%2FnRfKJkvCJM%2FdRqIOQTsP2ydeqkwr%2Bq5xgY6pgEROY64sSUdIuAFBlPb8xZEzwPeii9u4dNsPzTnyGz0p7zmSOO9gmURrTWAboosNhcJ17OvZ5udgCrsfU%2BtD1kjLeHrf32T6C4DtG6uYu6kdn%2FF0tV2C2oyU8Ds6dBe8rE7AeHgnDEfBBnkHGdKa9eawIBRABoONO4Bk6NN6bKDPhq%2F%2FsmIXx%2FH7vAzbvRKXPBrAHTXFBfwvLZVthzleHcoaPUNah1C&X-Amz-Signature=afbc8a6b636cccf1db49b58ebbdeb0af1f48298bb3d154745dec067eca5a1d07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

