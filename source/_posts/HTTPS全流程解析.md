---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46644AKDUEG%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAShc7hiAnnw2F%2BN6%2FKn5R10zWXiStN18HVO2wkrsKIrAiBTqX21Zrg7sWK3a2vJTlgiRT89fAF40lPghJd1O9AENir%2FAwhBEAAaDDYzNzQyMzE4MzgwNSIMwzkKEwRJpg3LcbVsKtwDTyyEBLMgWR70bNMt%2BfSuziWdPcVoDDn0aBJx%2Fxb0aq8b2%2FZy%2FJ48ibTzAnmbKf%2FVVxPL%2BELLbYXr%2F1yjUn0Y0L8QXFHkP%2F7TdwSZL686xvDaprJebpi7M0nNip%2FFuIBZCDv%2FSfohkohkLtLfIBR6111QUB0EaBkbJCsOEfgZv84X6AV9vIXszVLP0yr1SguSZMylxiPZT5zThP%2FB6gkoDewOymqSEax3AE%2FCBLhq7zGdmyfEgVVtfzcXEOCW5hr9%2F0lJPux2anJPETA8s8HKrxjCfTyXFWL3gWxkl2TeQopTdv9OBngIdp1DIAL%2FCt46n5nLtgy5DgpQ9BEqLNOVg9BtwH5ddFaaZuwq4GGaBIiPJX4Iay%2FmvY6p37j1MSDVIekkjb1rWs1W4SVyoRUzvwB4BULd2mvUouPWBfg9YB41ejrkWoy3DQLq17strpyy%2BdnFCsVP%2BaAQdpXD56AW9Ve%2B4PJXYbzTURMvCJbE04LuIzUEolm2fJI22iHeq3pmZiiOxNDiZmqKqkI9kbwshkrRY0zMgSOTtD0P4%2FOeRh8fWLNB6Wl%2Bc2IaqtCGB4PGs08uJHF40o4kPeemRIwR5pxAhr8YaF9aQKhiIQY66JVSPOX2VvwQYdgf%2B3gwk%2BGyxwY6pgH57W8jDkz1uBj%2FZ1zQEz3YLsw%2FEhghiya9YDmF2TRToO206li5rWJPiUq18jtU5e32kGz1B4%2BkjFepkV3bzsyCgFFKaIe6tB%2FTxfMlp%2BAuM%2BERC74bb06jpd5WCOzMDSp9kmEIx7VAvh8XIg4wK25pMoiotGkr5OTSeRElTADDVJOGPtSkSqx9kL8wWeaO941bTAURhosgJPEwumNCBsuHBn%2FfAidv&X-Amz-Signature=9e0d7a60866b525c8116adf3efad246b67b7a0d714a6923015a54ed25662d2e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

