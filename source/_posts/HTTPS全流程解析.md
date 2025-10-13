---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBRHIGWU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLNgICYSerIY3a7huxLKz1V2B554oLp0DF5cort9%2Fd%2BAIhANNzIT%2Ftbid75QQqlbD40ct5VUp22yxEunxbqQP0qUfLKv8DCEwQABoMNjM3NDIzMTgzODA1IgxSa3HSjE6%2FCErX4uEq3APLjmHZ4I%2BZZ7QKlcjBdaSzfuUv8yqpwCqjzln97JQhvkM4XrnLjerlst06JrDDDZjolB4JdI7VA9%2Fz83naCWv1PeTr%2BhJP5bKO9qR7mznL4OXAxYIq6e%2BUwH5evDkTWNOiQ2aqftR1M13rfErkE9yJ3EUcyXmpZaqeQBOfXL2oLdOKjJp4jJPilx6fPmZyEPrBjnviAfo6zkGvL1X99hQiErL6n%2FplyNNHxEvHT4gwTMxRnETZhfOWMHIK2eR42Gv%2F2WSPLuaktkb34shpGFnC%2Fqy52EXaFtjD9AjqfNvtEwvhsbvH7b40AUolfRdRgm3kgq%2Bh0sN%2BrS33W%2Bq06FGFsyj5Xn0Hj4mO2Ddj04Lfz7v1D%2BmS%2B3kzrfsPoLKdRCTV6C%2FH4jG26mk5nqrjtn4CN9Kot9QjtCK7g8vk%2FPNDtjzEp1j1S0kCf9ktbtoNe7T%2BaB0D6DM2%2BCNIFInLNxtSKp3aswf%2F89FzeoXUX%2FyqnMKw0qy4AXNOYSFo%2BOKkGrRWL8stXmKwBmQAU6AIMV3Sciow%2BX36CTXWwPLat7TreYTzwY3Oc7lzlNisGJKldsfTveAkX71n3iGJgw2F786IhU6zb8ZYIFetM7nMODEiEPK6UQyahSFm7zIZlzC3kLXHBjqkAfyKjG5RO3jTAfn0be00yuy4cF%2FFB78DrEtv4AMjdsJPB%2FWn5MPkn8PN2MGKSNYbGQhXrDzU5oB4LrnqxLJ%2B9v2ASwKJ5o1tM0hrcMgoGSkeHaFerLBgterqVGtOIh8VX%2BZDteSIpnP1GodwzljIrYa18cFYk4ROey6SLVDUv2LXTYsmJTREYdkO7Cqn9dHmx4fqSx0fYj1xuMf16RpOJlIs1dIJ&X-Amz-Signature=dc37b9f4ad814c74038c1be87eb24ef36fec1a470cb39a9f092dc2719bb3dd29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

