---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNRQKI6K%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIFjp%2Bic6SNt65bCR0DaME0DS5fvVEJPXNyzSinIA4VDRAiEAm3rdr9S%2FkBrQ%2B40HY%2FGWpeHPq977btqyhjRelCPh5wAq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDB6OTvD9%2BrFIO1DW4SrcA12OXcrs06fQ%2BztJMyvnZ3FwOcmY%2Bvzk3bl9v8kTFHgw5OsmDsqCf%2FZunQTpN8Pvbg6nPxs4OwYJR249FI2CnUkg5GoNBR4%2Bd8wXBExTlt5XPGqgsCIdUTaPwkzIrLEBjkpKTH4CZh5BghS%2FivN%2B83mFHRnsYIIlvoSNS25GkG8ORHvtw4l%2BFLVEn9fIlIhtZyv1mHKrh8zke1CxXSe0%2B%2FlbmJtwoASRDKkU0ZMws7KuGUW8MPu7QmRVVWApKf0imEgTRxMYZiWbtezkWMPxsumvGaBcBO%2BQlEIRh8wW1BBQba1yLZ3zq0LWcwsU2gtM%2BO15w5Q3ij%2B10S9AmR3jjEuHo4kmLNJW7r1C19mYay5%2FU6gOc%2BWKMf2qOAqbZuom8jf1FnHcPRTBE8a9KZZc3Kc1lnLTF6T8sh%2FtTzcRtc3M%2BDqAUSaPEbOY13a2saOfIgNVA71MI5LsmYl9MOZ8mm30GWN0PRk5Og5E3ddLwddxMIZzs8OHk2L%2FTmdO6KLc7wGBqT2ogKJ1VRUVM3XOwREV1JgwJ%2FwKZ6f4oXyiqKuAoce8ur9BRwyIKENsHod4Z83mEPlOTJ5wh7NBQaiRTrNlRluevBfuAFPQsYm0MVEifYgQlpphyvzAK2bbMI%2F%2F08gGOqUBoUVHejn144w4pQ2qqYMv1QsyTnNnhJm%2Fig2dQk9U778bf7u%2BnCafNi%2BOJNSr9TuPZz2OT6hCBoH4oPGhNLEe6246oHPhsD52kmdCf7kDU3hcQVTZF%2FCxIMflOwSDaUUugJU2hqtXFb3RWvg53skq4JOuViUj%2Fs9yTwhOH%2FA5142wuUrDTW918zYMw9fX0pphMcFj%2FNj2%2FmxOHI9kwWWMRuj1pkYK&X-Amz-Signature=fb57079842d3a9ab99f75c7f2d3ad49862536a139f3647460587bc7a71573798&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

