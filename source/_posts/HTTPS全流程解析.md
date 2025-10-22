---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPJL47F3%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T000055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQC4CqZB9Pl9RR4fLgiN64dXgXZML2BqWqRJRjtWNN5dfQIhAPqvbL1JVRJ4A7mmYvTQbzp7hHwJU9qYHXUO7kggSzB0Kv8DCCEQABoMNjM3NDIzMTgzODA1IgyR6Kk5FNVJKKxSuGMq3ANsDJ%2FgdQ0hmhYyD99OZlpTCtxZExlCyny0KowFbUxlNFUSZDM1KBp1iTxVC5LvZlD5fSgulik%2F%2F8puy2qi7maOCS2l94JTiDlFBY%2FJ31tCpo1V27h3GU273zJw95OvR5TSDeR2sVo%2FF3G5ob%2BMj6lCeyGGTQuOiJDoYhEss2VIkbOSUGDSlRJqVHBExjo42ERe4N4%2FpgAe05qtUcsJxBxNTo0c0GYzdFx4naKuautb7DYBW98A2EZ2FNLpwknMMoOlLrrextw%2Bjo4Mu%2FsphOMEEvgGb1EIiqgNhMvfYKmB2cT8wFaUX6RDzv51ylOiPeDkpEQP4LAB5PurDY2xLhSH1bgcOl5nzzpehH6zL%2F0s1Zcv5VBb7%2FBEC9Z8p3vNbIkAEQlb177iFzCALnEM5nMcJilMMn8%2F6JAq%2FlxxvzYakNFEhfeG8eqfBYMiz%2BhN%2BSsveS6rFxt6fJBOxu8yuCqEdBcg7iXadwSKGqZwptE1RKJgPKmUHdqsN38S%2FUv%2FUyVFABSC2HqYVWD9iE%2BuIYNuGrcBfWQ53%2FClOxSEw%2FoqOAkmhBzGD7jeIfS2FfnVFxlqbj50nS%2FDI%2F0wYrgdXynasTr2KS6wFUD9tJ4tQ%2FpBOsZpHHD4mNWmm4nI%2FzDKt%2BDHBjqkAWXmGPZ2lRcRXJwIDawTNH536qWMEeOpIxa2LiTGtee2yFaQ8FMeUTX%2FZutI5PMrsvDvCOlsCun25XMrzLA4rJUbS%2B6grsWDev6eOog9qiKVq2AGsb%2Bujtso9j3g9vMd3%2BYf0ty3xcIWqRtM9WAo5ccLkN56Z3VtIpBI1e2EqIrrc9wBk8egRaeOKr8tYMOy29WXE4IUFuI6QyItnAX3e12Am22t&X-Amz-Signature=79755345782eeb180f2e52edd7448262029caaf13a034baa2c443c415cf9bb98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

