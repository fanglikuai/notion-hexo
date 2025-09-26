---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPLMATDG%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQD2gAE2K8lTYoS%2BaEDDqX02FNLhKF9KzsVzY%2FazhM8iNwIgcRjTJIu8eY%2B37SJVnfflGF7y2HrQBBG%2B4byNNK0xoBQqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9kZ3XyADciglqwRSrcA5Fv5Zvwt38ldaclNYixZWo4HoJP%2Bia4ZZDSX%2Bg%2BtBglAq1YvxUuZNhxIGRbvwAdMEizQzu2WbBO3hqCL0oUeQjFn1GGRKu1D3FQugpXlp5Cl04y6g2R6tVZ%2FGhlVsa5RdO28aeActN4M34acTA%2FQyRPkmx3iwKJuAh0Gih2b%2BnP5vmtvXc246bRxz0bKF09AHybGW3v3BO0G1LmrM4mrMY2yda%2FzsqFam%2FSchYH7fvQVyBYIAlB8KQKURT49UxrCMwKvsI7I7o9asFQ%2FWZ49XXEoGJm6SiPYJVDwfj50OUxk5DWEiGY%2Ba3xe1axNy6u6769tN9FhFS8RiqTWZ209StXMeMw639u47MIQaX%2FhlWYT4mhOmiaSelcLqQKhsf8c0QGDF2xSa8rwm%2FwZDN75Kil1LOi46fR%2FNpxNwHCjDCzPnazZdjsNuZ4Fxn5e0hNlM1Mn8EFDpPPWWCoePWhkmVdK4dFOVbKoodL3fdLEpsAxvQYU5A9Fw%2Fj8gAVmDNz0XB0PGB2jpoV7%2BG2ewXfAUXyzWkYKiPxr6Tz0NggFdiU6Y472PdseQofiLxBPapX72CJx9KvyQC1I6EBac8TzukImO8lPC9mInNgbI7x8%2B1oqaN9nHvUKhEW1N9hMN3n2sYGOqUBRe%2B93iysnO5aa4wgnvE9flcc4NxF3kwLi6CUgF1k030ga2ty41UEu4mPkN7NFx6UqMfv%2BLjfraLncVo%2F7%2Fw7yy4X10mn8CnymsVna7mQ0kCmWaLBmRCEL7aqYxYpdSzgTxFb1QxSq1z%2BmbJFJge7oi0dEiLoiJ%2B3mQu%2BK%2BZtQxVx%2FtW6X83gHA3cpQPVei%2B3Ww6wfRmGVZEsCWob%2B0SyRrvrBPlg&X-Amz-Signature=f709fca4eb4ff010bb3923df0bcbed103e4c005ebdaeeb26bb48836cd9674249&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

