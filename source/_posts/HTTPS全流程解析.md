---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SITPB572%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T090056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDYYQ2r1Affc2N38Bm7xfXup2idm8HFjx6TNMi2SGhV5wIgWm7FtwupgUSs9Hb3Y%2BACzQrPAiUPyRDMFBBIsUsUd7sq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDG6sgehEWA%2FwE6bpICrcA9PK7%2BIfQ8wuB3RRMBss1kdFFkNc3pR18tzTwB12nf7nLvuDiLfgGgbeMB9ykF%2BwPMPC36hEnoAxiaMqKddHPZUy4l3JE8A57FvV9jdcEqYoLPE4AWXPSHR79088RqWRMUR3D5hvU27Eh%2FKOA5GXo1KDOs5iUTODjA%2B7CcuoxjwQMBx6lTWcXCW%2F0%2BrPoPvjq3lQRIXLeyQI%2BVq6SIijB7E3eSt5b66VCoRVyEqacqNrSdUIrXMBPtwZoykp9FNLrnpdShDh8DhIvOphXHpSg4eRW%2BOzeXWfhqWYu6QkummZnLvDprYhlHz%2B%2BRGu2hxNkg10J3q27gHTF%2F62b1ffMnS22HUwhR9wyno6s3otjaERlTggaudNmid6Qt7eL53DxCJEK4LjQc9NzSHy7T202h32I2pR6R6ZmJPLOMMSc%2Fsn5VfI%2BDyQTZoBGKgho9O3L9of9F4lcgGyQyzIsIrPnz428HbWrdVyKkU%2FHXkBnyGQiPW%2FPLIcfDmtZgfdP5c4QY05AxHBbPmOUGMOATt%2FKRFeejifiYilHFydOzpZ057TAzHDLQ1RDiMvm6gELLZOZK%2B2RhhQ3vlUE0WOO6zoOryLpkhod6tis3o5JNUj0NVyALHXOBZUmZ7%2B%2B2fmMNeXi8kGOqUBV6wJSTNu1jf9MgD%2Fu6zH5q8rFmSyt3Dnz%2Fp1h6XiyHgDrZFLQ4KvvjlqaWl1Uo0CO9i1k%2BojqpTbH0wChyssPxGqQ1JsMSbNF%2FDfSTQ6mu5JI2BYEccDagoH%2Fpkjx3fWmA69FaxnyHef%2B%2Bl4NF9VAYy6D4Rkhb9FQW4sXQd5l5MusrST%2FFv48puU%2FWMhJnOYAWT9cDbDxv5Gq9IhJlaE39ocgdNi&X-Amz-Signature=79833aced3d9c30b6dbcd94256dbb016059fde6b3068283f99e200e72855b1aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

