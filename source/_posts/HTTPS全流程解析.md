---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662HCSQQ2A%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJIMEYCIQDHrag5Ka4Hm2R%2Fuv4lLgXTHFFYCibuK3b9jT%2BgXQx19gIhALAHJJVz6mZvWyo%2Fr6JT8JwfqDctql7kjV2EHJQ9oeLoKogECK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww17CSBOb%2FkmAZxn0q3ANPhnfWCNPbXQQrHMTyVO54mzMtZxZu%2FXqhiySq9ZhiNR%2FGCdY5qNLAQ725SbUa8JO7INYPcdja1l7wRquM185yGuOZQDWNoPd3pTUF4a5%2B%2Bi3M8SzzVPbeXX%2BNueoW%2Bwb3twNbkdTa6wCjCmMvXmLPIz2yMhV6qm1Jd%2B8tiJPy1RijqSpaGJDBipHvaLPdeLmsVwOxM0LmDpVZCr1NY09sXHk3GckzEiwavXZNTL%2BilpGawRsD41HQw58pKOxu0H8HE%2BRoFkUTfP727xjcPeQN2EGyENkVCcLn3w2ETUeIJPrnma3%2B%2BYdou6glPrkLECzjQAzNizatOihzTWYScZAbgYr3IxhhuQ0nXglZu%2F6wCS55VBx2K9GUvNNFtUz2rcZwvRNwTSFVCySN7qQ97vNUOXJJOOadaa8Ok1xga%2BLksrURXie5dFH2CJYKoYV40iD7aeih316JIf%2FtCBUISAmyl7TFgcOVTw8oQ4d64PUuEpl71NZP7EogfQAQRLCEpzsaFySP8iUm0qRmOAaEOfPI6T6jCaYJsZE0G4dOsT%2BW44db7KMZFYObSDm8j9Jzi5%2B0jZ%2BfI4keRw%2F7YNqpN864Uw%2BCt0ZGJXdzr5wTYM28ye5WtvVMA5nl7Tf8aTCg%2BMrHBjqkAZ16JDKTmDUOstxI47cRoOm16AsztnoR8%2Fjn3EF09l%2F1PyowAYLhbsb2Zuz%2BSYjDNpo0QoCYsFDW4MiNscp9Sy2Lxpi%2F0WhDXIhf5AMhtUpBSbo0%2B9VTAv18F33mdaqMh%2B1cGu7Ljq5YDJEraaZQmhrhYiaRmw0%2FekDwJzkK0YYwuABeaXtczRbNAwTubGRvcgp7UH7byNF4E%2F50ezFQgKQAHuYL&X-Amz-Signature=99c74bba807f0942aea6b7e8a057ca54c37a92d50a8523313ec55c7b435b2aff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

