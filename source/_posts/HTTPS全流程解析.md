---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CGAHXDW%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCtBfi4%2BHC2YOHqBTV3LsqCObGIhQqFTC62LnY6UoYrQIgTsiw9LG%2F04Rle7GqUCggeXq3UkbwcD9mZpoJlhMDtRAq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDGEjgCVrtSvOU0DDIircA5dZO96TvL8ntKR%2BoBokIr8ViFlecAmzm%2BtG2rUz%2BIZPkJ%2Berr9CP%2FL1WWpj0w3SAWzIUpeUMv%2BZrNeaONmwrSHKzc3iPCWk86m9iQvkvVnURp2Dos4AbDmPey9QwsRe2dLNUVs0c8VI74GFiSpI%2FQxcPWF%2B5vBYedNtufYOYgreL4Yk4ZI1yunD2rVhF%2BTwcrbaRiLC8Q7lSsCHWOK6yPtEhQAd13Cm%2BAwO4PDBIn5pjiS8RLVrz0VQhFyzoyt%2F74ehF71PssDiCaTHPSefPPlONEEU1Mts9rmZrCuXX%2B3RwnoEJmkDt7dpPLu9Q7dtSTiMvu6iostpeURX%2FiiJhSTbHjnEihgaIbNmKNOhjLf4sQizt1e%2F%2BY4l%2BMLr6syx%2BOkizcs8g1Wfp0F4y73cnIK00olYUzwMRNy4m4nntoFJsvPsFKpCdvFjtrQUHHg%2BgT4Gxo3hBbbxALqix5kVFiwkO2RrvOv3uRSGHq%2BqP5BwLiL8HiIdpgYdGIjcH0GLF1dKEIjz7r6OniGnRu0FFJXuAatIGmwhfP992jgPTrHfbefHoBwJ%2FCk32CC94ukI0Oby7u054tH6shE0FZ4xY85B9%2Fambqax7UZ%2Brt0L2E6UNxiUobxi48nvU16bMLjf28gGOqUBelnFTWAAgN1vUJ4c2ZeJVCCyNRNUs6FuyBWIKxRj%2FO4QEpPZOcHwWzhmhE42rTWhnQ9ciLVySsejsRlunTkav4woIvSmauP2d52K9LiND4pJzPFp3lcRiQenfbY1BOtxty2M%2BsGSIQRUOZxF8INyDftsh8b%2Bkxv9s6E%2FIkELLNpvb9zEM3wH9bdSONbqJIFiaVKCbzLRjQ%2FFPk%2F9udFEx29Ug%2Bi6&X-Amz-Signature=9ac768a3e5ef9bd2061a4d017ef1870c76c11ba1d37d40b56322cb3f8ab1a3a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

