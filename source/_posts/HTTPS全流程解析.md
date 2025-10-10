---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q72H3M3G%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIBG8o9h5wyYn0J%2BUUE7Cn9ok1f4AUc7TyF2R4u9nD4jDAiEA9%2Bdry9LzXjOrdxt%2BprgKZGigBqCS6bIh%2FMV%2FcXtz8lYqiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVN90eqhC5AP4ngxyrcA%2FBwLltON1%2FvAONSMn8vOYzl5UmFU%2BLYNyIbvvpONd3AO7fR9Udcbr4FjZmYbSxr8wpXiTkMRPGSEycalo7x7A5YF%2FnICEVlG0yFuvci%2FXRE7tIbQqow0ZRnjMeY58QOwkBpfOKx0gT48qyg4gfitVkgAwwXYaZLXLGSg9bHFEaR5Tna6wtDPNxVAaNdq3c%2FOc8%2F%2F8UCq2M16Zrqb4%2BXQe69oaWIcI92PwclgO1dyKi%2FoAWhc2gnz1wruk33QFs5tOnSeU2joyZ2T%2BsRiwpRyS%2BBpYw7rOBc%2BW8XDAiPWivBHg4%2BlsSsfi2tjQnCXz%2FJz0aZXBBn%2BU7gRbUX72EbmaNo72dSz31UaDpUb1IE%2BPQcfiedvyQBwnHSMFDhdnwT9Q%2Fxc61rfoR4Ql%2FC1XiM2owq1NTSkQiVI%2FlHPosqg5XCUL3M8pbxHWCjJf2el7LrwE1JeTFNOCErqKbzOTrWGrXw3sgiqFdBqlATFy6jE97hortj1bw2h9JKm%2FYrgMagex8CDVPba209DVMkeR0TrPKF0rY9hyK77knYSeReMHFsHqE%2FxI%2BsV5lO1zSOUueH7TyUzt1KK3rTBxdW1g1fkfFkpy7gful9hhBxw9pD0O37P87P%2BnOLbH5TzzFuMNOeo8cGOqUBHxVJe%2BloIZcVAwn0fUIlOQpYtYN6dctMAwfqK17%2B0XYTpD1N49OSTaoXNYhlBBFPfkpRJCialgG5a2Dgtp6fbNk0Cq1SdsHZSHHhsG4UGLtZOLUU0dwoownbgqE58pl7HZleuiSk2ewCWq8UsuulPudwnlJkq72N%2FJzDJMdpMSACGzIYodD2zgKlqqTquZiBBU2gd1cS71x5ByA0n3TgaWIO6jQY&X-Amz-Signature=5e21cd6752cfb1b8ffd40ce31809bb8d7c72cc86c4360d1e17273e6891ba2a33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

