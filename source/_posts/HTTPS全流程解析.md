---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXBQP22H%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIENZM4j5yxwB74zFPmPQCksOziv%2Fcbll4wEEf9n%2Bg9EAAiAZU8c3iWUyLFpFsAKuXphbzvpO2CnPrG7WXyDBFkOtCCqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbAzPkPEqTPoWKbO%2FKtwDjMV20BAuFw6LGA0YNanyUys%2B8mkMMmlXssxQyB%2FvRfdWiJdbm6yxe8U2uvSKLYkiXu34la%2FyfIkqNIkHiQ5QYr2n9ToSmYdvQJu9VKAlrH%2B%2BbBQbpYUrVpEuCl97TZymwXlyrfr5YdpNBzDwmwYngeuGUfN6LPCYFBEES%2Ffa7c3u9btQykJzuRJce%2F13oLbfLEXrwl8WEZN2jfDAbey5kXKnZWYnWyQOMHgTdxzR4H7vJWL5ixCmyLE%2Fyuzvmyunwjz06IO1YeA89vUIt1dB%2F0YMfpVB5yJD8BBxcBZTMvWK%2BDPLtLdUkaivT3%2B%2F5iLJaU2jEc6QQ2ocwg0hjNYqrarh0EIcpW26f%2FFmsx%2F91F5T5BODOzTgvWREPdi8tjhsDFYGQBsqsCO5fq8j9QhYX5U%2Bqw36%2FQlv4Fb%2FpxbJbFPBZlNCIStgiI0%2BQ82%2B%2BX99Lh%2Btfe1DNd3Ip3zyTzCLc%2F4gn0rCX2gAnq0RN64%2BGrfodw%2FCyY4BDlkF7wOhvtTz%2FuEEt%2BV7MtdXOo9bbI4b%2FCTJddbm2O6GybeD7ishkKg6anfEKDw67AgRn4CwjYY13C6QMLC2S7sDfA27UpxGxO04KSDK0DA%2FW9tpHkAkjlWcibwB%2BWfhNlb%2BGccwqpLqyAY6pgFDoK70Z9ulJkbMYi%2F6h%2B%2BEudTo7dr7SXQn1MVYFftQ5xgnDREqCWYLxjKas6ihOrZIzntoYnnl4l8%2FDTh6YDRtae7LUTYpwNpvLcTu%2B%2BYUmmdTC%2FtfIjdX7n1zKwpxvHOU%2FGFKDfKrEnxG%2BuzYuT9S97YU5VFn%2BRlrhRP7GzDL5tsvvovEpGfaoGCCxncs8CZzJVCwRHvO5X0qqmJ699YC7nxqzBNV&X-Amz-Signature=8672b1f21d5b753aac8a36e1dcfa8095e02a7b8bc083416b417db45feaf8111e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

