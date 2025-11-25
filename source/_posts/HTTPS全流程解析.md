---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SZLWJDQ%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBZZas9VgP%2Bkdcn2kg7vpUYNGPxZMuU8oOcXP%2FbQESI7AiEA%2BaE3RSN6QcHWmapQh1AKJ3TM69Vth7WhSR4DYnRLD3oq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDLQalkTrocyCpon%2B2CrcA9WeT9ZBt4gJS%2FPVby3hTXdxGUgCOYedhdmprdeIIHCzvBb2UBLWcHYvhEzkg8m%2F%2Bjci569sCFYXm%2BUK9GPU4fL3NaJq%2Fz2fb33Ed0l%2BW%2BHDryAIKv8pEQAdLcMWzaGRnLvra0VsEf%2BF73t%2FA%2FHH3Zr5Ke8e2n6elzgBQZYAj4Yzvum%2B5tcu%2Fu8HaxUd5uH6d6Z1Eztt73J3SwTGyVLbkHA5LYoV3thslOZBqVDBV%2BwaI2xhgGZDahUFFbnXtsbBrqAI88GbusZqsNNwo87Xtz5ZdhmKWfsHLCZYRV3e1Sv2UCkdxxHidunlJzQ5G1TWjMD2QCxSjUE%2FAZTNKZRttmXM3rBciHroYr6wRjxFOTjFDbWl7qEIM1VGDAw80Pht6PLx0wZE2%2FynwhqrIFjr7g7iOptVVMwdnUADoy3sosDNE3kGcGF3t8K2ht7cYISIoReslfeD9xS6%2B17b2uV1oCwRddkn200tgyDrhP303szComad9yZakzPbaSla1rxzMXzj3YwiMhP4iGOZmiKMNjMrxsHPtdGWqeIMwlWdkZ1f9zgbGCyIwrx9wLYM7D%2FlSx6MsEMCZOfe3uucpmWN9UgN1ufAM%2FCk7Qs8CIAC9TXKhdoWKETpONp7Ab7mMMmElskGOqUBJewfJ3GwfvBBeIdyiGnZdlFCges14x7muDcvl3uu3l3a2WLoSB%2Ft%2FjCPAkRVrT8hvIIhZ0x1%2Bi6IA15KL%2BrDkDZfuNoPJBpaA7XcSc3%2B%2FlITmy05ujasa3hAP3%2F8d8CTh8fFDcjQTeXKGl5VGhz5KsQY40WyCKgH1kWiM2SF2V4%2BE2Yr7cGfN6ENgJdzA3YPNqg3M5rXSSWzJDAghHO1ay7sWkGq&X-Amz-Signature=35437ce1ac48a39a5f73644ca0881afed60964a3b1a98c241b0b2dc8383375a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

