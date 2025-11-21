---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YVS2WFD%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIFqmPENWcT3fkxQ8Qt9oK3Wqh0Ky%2BoMMksSUgvK7Mlu1AiEAygJOqP%2BFVH1bAoLmWN%2FHFqFbcreANEzw%2F2pxBpVy0Hwq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDCWOUhLUGCku9YGT5CrcAwUD485lnNYBhzAgDbfbI3TMaBwfUNnVwx%2FAh7BR5J%2FnM3W0tJfoVDegFke1yGZ6A9RwovWIi5HoR%2BXJr2Dn0glB3bvpV7pxtFzPmhF7hJekqIUpsW3efz%2FseRRNpqzkpruZDwL3MpJimcWTFklJVIghWGRqV1VnLliIsBtrIWxPY2M4C271KbFTZ%2FQfSJhi3tU1F26ndU1vnvldn%2FWQUx3zJByauU6BHwTzKzbhBcvrJZ6ak4mhHDAoFIiQ%2FSm7HyIhuJNmDLb3NsH57i%2F3zXVt4lcvE2yZVz8cwj5ggmKDJ6RF6eMcXTJGMk41d0YC6ytaFHPqfOERZPhqChRlFk81JpbbGP0MpaU2RVnORGPFGDyorBgKTZx%2FNmKwbkqBFlB7ZOYPqeswdGfho7VFHkuyA%2BXAjtAe%2FGT6T5oXmm1%2BzXFLq8Rj3Ifi2xAPICjbBQ8woywOrFxR0HT7xwVam6%2BNI7rplMonROVFa32WnlJRGgQtLVzPcCfNrkhQwwJOs6g9ibCMeGr3gZTYfwUVSOIuadrOy6OBuDX7ib4Ye%2FaH%2BnQxgaYbqP6v2%2FdoTkJHzs4XxbkEcVuONmurDrLmiNTxg5gIMiXcN0E5%2FtyL2a8RXMAIuycSyOV9bN0aMITG%2FsgGOqUBMsSCw7TwiXtVBeK%2BAQZKFbrlnGM6sttXO%2Btz8ZuOSMZRhkicWEpC8oZ1yCEs%2FyqHahNYHh36SBIlWSX5Z29Hzo8K8dM0WLBX%2Fx355GCjIw8eyFPUcwKpu8BTf8b6KLvZTEzZ1jmOfyu3C6PWYCSZ9Y4kEpWDuaX0sEWD81fRASRYw71EpstW9yK6W%2BgFijGbZj9KCedR4W2D87dvTzgwlj8KxKO0&X-Amz-Signature=8cc12abbb1acf49e22a53e256fafefa3092c868ff5e38752791a029c3df8226d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

