---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665STVDGUS%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQDl0TP2WRt87UTFmdQZUNPdSNMFiZjWP9XBLlYxIpcxvgIgOCEOWjNbbO0srxI2TzoW6ecLYbbil3gfWhtIFEE20a0qiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMtQxSz4ZyR2u%2F%2FmCrcA%2BaHjBFMuHo3AHsjVGPvf%2FlZxRF6JunZwwVxOXPIYhTcDZgoMTi1DWVscl5fTgStlvhHShJKbvpL%2B6U9Y4R9%2Fn4cKr6bmIeJw3SA4S1I2LL%2BvBQlj5GwJv0Nf%2BgPMeewHxxh8j%2FVtqp1BTR%2F2yn7zTv%2Ff2VQk9lV0FWBBQSzFjfK0T9jsSmLfJe1KoqrLxPsR45ajFd7zuJe6cXEgzCcSRUHyQ0KSAI3K8Fy5bzOMs8HcPBIYI7kaOsWu9zVOiq2avcoQsKiBd03%2FHuw5Yk8HRYwi2UCmqEUzhDHNY4rSaP7yxU7%2FVTTy2UD6nvSXTTsAkrtN8WfDVNQI6b4znMb%2Fa2D3RZJENkYWVyq7MyLp47tqu7BJAc4ONheJ793teTntqdz5AJA7k5djcGVdmobA5%2B1y8XEloxyyrNQ%2FoR0Zkl9wtI4t6zHNkRIsQ7aV4TAlrA5xN99fmceKW2GXe4JhcpvsQwQMAfm9b4JnWHfIfzdyleQaOQ%2FC6Y2VHMVug%2FHlqAXzsugxE5ONgvyq6CeMJzdxHgMz8TrnvRzGWW3izHvmFCmBtOtXZD5bribHpuN06Qhk4TQlccq6gFkbycJ1TNW2P%2B4rvQf2dgtiXuf%2F3UCHPkKl73ADewdJzOMMMW%2F7sYGOqUBpfnSTt8xPRzf2U7catW6xacb%2BkLFSmvzCCO5wPtmJ7mJBEndq0TgRg8qtd757Vnk8L37ujnqSHSf667HPkRU0odHRgTc7H36%2Bt8bIz0oRujdkYqj79bdLuWEyH3aACHOX5jgCbr6KQB%2BbAsJg4MpFOfCbFiIkTWFC347D%2BKY8AP9XgJZqgmuIAXyTrggNjAhM0HzBtamnA5SH%2F8AX8EkIR5Y8FQq&X-Amz-Signature=0f2e45769f6fffb7e6a7fa79ce5c78111e480d7246d4a69a9170b38407ccf012&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

