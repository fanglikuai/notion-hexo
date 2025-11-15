---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE66G5RS%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAhm8RLrW9pldjIvBGuWfUZz3ojZU8tGAA09o7bwQ0QQIhAJ%2FQD4D2%2B0AlcoG%2Bc6kAtUOkeydri593g4vFrpSlFVN7Kv8DCHoQABoMNjM3NDIzMTgzODA1IgwNTFXtKvEqssW6a%2FAq3AP1ns3q%2FIYtSvz0KmzwBgu%2BV7gwwOJg4NHU%2FypbgaBHNc5oCBU2g3rO%2FHqOc%2B1m%2B94n6kedt9TptqJ470pNNbtocdqhMMuBQQlb1Cs9IEOMHqRisMShRULM62bsK8Lk2Zbc1kimQBORMuOfjRS3FHqYu0Rfzcm86bancI03YqBBh0v2j%2Fpc%2FgNCmJjaqSVQcA5%2BsDHadk2Pc5b6WH8b2XihkqNEJRySXa2iYo05MAp1m9y1gYFyfcjy6vmyT7x%2FpPm3RSloAKqTzus4An1Z7xmVzLazLQUFVWMOImDrBI2CZpYSmc70hgA4uTlXsJkwawF67LIMgQ5DRfYx0RQmhYLS6f1AA8fVCnmGRf5VovtuP54njNoUle2mn%2BQaBTf%2F40ktL%2Fk4QsLH%2FPqSmns9f%2BG78zTGDPH%2FDYcbuTvAkCIurOrX0tBSq4SYt15pCMPoUR7I8tzq0Y24dt6KGiFMYwIuCX7MuzOJR1ofQJv2xzb28xx0EMNfIWjd3iCuAqeXE7k9c9juAYXza5D8Vy%2F2%2BFJL3Y%2FSJizys9x63BPwnO0tWer%2BrYL6E1S3%2FTDAfUOUtxF0YVXLsC9yNlgClnMqAORf1HJbZrezloYGzitVocwPJS3MeZE6vJdVDaNc1zChg%2BHIBjqkAaT1LKNsIbi%2FKC%2F8%2F5058h%2BqigRi19DWq32V5yoOdpAfO8CdM7VfhmFUrxG%2Bn4PCZtETOt%2Fx%2BYDF4o20drclVHyf4VUIRo7j2Sui039ot5zQcbjUbG9t7l1Iu28RZ0Cwlj62o%2BKe5NAZC%2BTHLKb8wOSf9lN9%2F8zvo38YuV%2BUVA0xSNdmOZZsCeZhabVmcpSaAE5517Qxo9ZHMzfa%2BgAFxVDfwgyt&X-Amz-Signature=7b8d5fb3fe86cbe65ab989a0e67ec06b5877bcffd64552b9e69f269e0ad1b70b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

