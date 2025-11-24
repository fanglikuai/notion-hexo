---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OH5X4KM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T210054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICSOnr8dchXqynrdLKafOZYh%2FQiHadANCuYTFaWR8iLKAiEA%2BTwAQb9V8A%2FvR93KdAlLr6LvJhjxi4PqVYf5iZWbuSYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDIJMMQu%2Bsqf6KiNpASrcA0uUNFBGgOVR28KyXKz5WHscKbp2%2B8kWzPXrRZtQuo%2BHPdsKDsgdPH1vnYQ7vJye8fNaMFh%2BcSegwOcr8Q7G%2FL7Kry3HN8LFfzBsS%2FKYPN0jxmV6iAxEt1sJfl%2FCG3hcxnWhqMpIY1ANmCmjXUUtvBmhfLVfZNyAIMJ6yGzjrZ%2FDOMCjQ5xuRRdsP7zMCybmlc19zwSUa7KWRCGlLFWgoMiaDJK3SZvlzW1Nn0Vl1qAvGr5cxtrFdGnP6TcntEVDxWhVBNqPx1zcCosfiwURm%2BtCJdWbOkfuSYneL4KHwTrFXLL3cjezS1OUEzljhnNP9AvdxMajbqZceVJc9%2BSjoCEm2JBcJ3wdgNV36QClWBmk7jFvAnMp6ZDQfubRYacunIKXxN3gAbv4J5cwSi0GuKJVAkGt3Kr%2F%2BS4xfpCApV46BO%2BblpMRGUhWE3%2FZxVCNHz5qVA5ITnja2c8iQL2%2BVUrBKZJ2pLUNfTBJQvPeHsdrEchMB%2BVsrxrWMA1JdjvsDN0Lzr%2BREIdlYILtPDlXMG6uSGRjhSHFZ4xxRxnG3OgeJDBUHB5ts5AwywJI2F7Hn4wWxrfu0HVbr8Ee4PBeA6osndj7JUrJu230CpstACdOqtSnQcvJRwygweuJMLH2kskGOqUBOOwN5y%2FNag%2FENjuuLwex%2BCeAQG2X0PhoIxT0vtU2Bu39ZzErjTOSgMvaD75pnSKl428H8xkBY%2FXo1m9sj%2F6HJOBUOVwo0UflXMxS8aUtNmcL%2FMlUBzS1YT6g58TrJ0fhlesZrq%2Fv2dd3HPsvK%2FCqYe%2FFPfhpDlGSA3VG8ftxMfMoHLoan%2FZgtTA3tcfcHLNXt2uYFVrAGNsq3LhBi%2FlOihriWr2K&X-Amz-Signature=f253d1caec457dff7f9861a25aedc06487bc01f62c09e411657a474a7678d8b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

