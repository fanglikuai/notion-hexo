---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPKWOVDI%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB2kJXLTs8ATIfjFmMV4Qb3crn6bAU%2B%2BBtG8Yjs%2BbT6%2BAiEA4gRpo%2FHR0BILkt%2BvnBs%2FyMh3JMkoEQhlPAAphi%2BXnL4qiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjZjY7WfqiLVNrNPircAw8dX4yTjtfJTm1Q8wKF5iwNVZ1oc0%2Bm6fPg9UaF5V2RWVKP%2FFqmdm%2FouvOHhknMy1ld5DEqH%2F9VXGLONlyEsNsJbfBX5R%2Bp4jwv8YC3lzBFFxk2iG6bi6GujsKYRwenAi4cJSadFX4NIc1smLK1YcHhwJTrtmQ0OxM8DJuSEwpb3BZtCVkbbPXFmr8EYAVasTIJc0X7S8BhvY%2FAI1MtXLlgaN4MQGWAZQbioURUy5YLegW63nyP8GWLTPLka7s5FriXrxU%2F%2FrvRUHDvvPYrJo%2F0IoQSA86KBAJhOt6xkviQZsqjGQT2a1o5BNPktSNojrf3vpw4CkWgYi3QHj2VKUNPDR11AJYihI%2Bqvss%2B1UlRKqYuOQgwlKhLc8lP%2BGc3PWRgl0OQ1FE%2Bp7X2PBBIKbhzLmh33zn%2Fp8O3vF%2BveQ%2BdJtjPwJCUqI9C05n5rKq1Ilnax7kcjKO6%2FYf%2BQq5bsSPgjslJUxN5u0PiQB8QWtRPP8n8CEAlaOhneqkBLoLOfPgyU3LyIdkxXzn%2F46SaQLOXiu0NzBnEvQMDtb0qqLPUGErye1gxzqNuy3Pwmusdhf2W3hD6ZaATa232uYRe2klHtvPd25EBKcSaJ5b7MbDKRRG%2Fhar5ghtEMzk%2FMOre58gGOqUBolU8PTyvlkqY1ewI5Lt1d%2BMyBMyDRgWmnsqxrH6Lzhij2w4om6JUftVmGfcSPf9eTKm31Xkyjx8fjv0IXdIOXLcZZHhRu599O9fTzVCPSmFjb%2F8afl%2Fi1LMEwIXsH0Fr0Q5cdW5Urz%2BOcdWLokx8tFcQRJlonTI5CwITp7Zs9kKCNVNRTVAZ4bKV1oS7jVHLvbBowtkCSSWkhl%2F03fxubBMFYCfD&X-Amz-Signature=53d7c5e4ade2fe5d8503756fcb8df898b6230c45b5f409dcfd37bd5ca830a599&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

