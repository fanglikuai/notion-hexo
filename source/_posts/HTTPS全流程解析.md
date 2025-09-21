---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663C3S2JXH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAsh9bW8udsh2jRVJ4wnGv0JX9ZGrvNkT7ybzHweY9%2FXAiA9KmLYhw40NdbJkC3eMqV%2Bgxn8hFYzXnA0P9JPdmHJEyqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM38Rx0CMerhJNxOw6KtwDn%2FLpD61c3e8uASyDb9N%2BhFoQsjcPv02UkV%2Bss5kWmuaYcLI%2BCkNu3yZuWg%2FgQ8zIN%2Ff%2BosO4BbyHQhKRdMkKhQVHAurojVhMsjF9Rxy52%2Bgn%2Fkimac5Qx%2FJeGVnG3A9Gh0jpFzz64wxvgknUSHiu%2FwX2KuzkJZlD6WPHGHc9EnyNraBhMP3yKA2L9z3wI%2Btb2%2BvtJFSsIRhfD6%2FHEejkmyYvST%2BLK4zP%2BT9CcogcevOo7g7H7rKnnHXv%2FNN7z66euuw7MZZBCP8RJMHzU%2ByGKrADsdcpEjuTllCfdmLpje1f7cp2yZEn74mmuoxJFC2MaFd93igUYkscEP6xolf0%2BoBX4bGpLttG5HcsA96Lbmg3vx3ucnwCAz4%2BzrQJgyBtSJ8rdInzFLnUFaZvjKZancIkEaZv%2FcNstG0e6shFPNO1yUPa9N%2FxDXClBPa1alQf4wxuZx1CGH0CUV9FjQS%2BMRLUUYAgCWwibM0d%2BjkAh3%2B%2BO2qhOTw6fFk4i7PBkJcy0kQHQ9z4t1mz2IhVMZvTYWe1c8mm%2FrvpfrcCGAo3yMmvIJpg3yT%2FJYH1rYZwKXgJ1dBE4uvkC1Oppfv6l65rQbzlLxkVVK4g2oRGJGwq5phOApuB%2BvH7WWfdc%2F8w1%2F69xgY6pgGM6yGfuSrzolfAzPVbgWqW4rEkLarqlVfxi4%2BafGBaFLS1INIfEnfJofez5ytK9Sbem9nL4omUxc5Q5lT%2F20VO0JD61%2B6zMOr7ph2DNefdyJA9AsVKlJNCp1YSeyX0nyxSeLkb3ueLRvRl65QQVGp9%2FOBYrAr71CBv4txMQf%2BxNMxFlFRcfSbXhzXjnobOQpJCiR8go1Wnh3T7BaQ7pOe9PN1buqGc&X-Amz-Signature=2de3daf8c1cc89a4e399484337ff4f3ed3c528e9f875add7132f5e0f8010ec73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

