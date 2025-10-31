---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG6DYVK4%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCU0WKfi3v8KiLLndl5aodNJ0Q3r1z%2FFlNJ0e2jGBUn7AIhAKARVAI3u%2FwrzVhRWdTW3Zmf36hRtZO6Gk011V8xo3BNKv8DCBEQABoMNjM3NDIzMTgzODA1IgyE7I%2B1AOuHkb7LBbIq3ANJ%2Flb2kJs%2FSw3Gev6mtWc2fUKaa4VUEeSu5etAhE1%2BquYEc5tVIAPXu6hCIATUU17oZRmryJ1gvsyVBuxOZ8tzgGacqn4xsciaZoG8lZsdqP%2FAEN8Wm4Ym%2B2JK9ErzWndrmVtIuMrGCabsflZxL5K%2FjCbjyJAU3t%2Blc7Q71OEimOBw6lG77WoU%2FfCJtzCyhK8Gn20qgyM9YTbyMUlMX2sHcDPlzI04JpM4OIvyH1aLivqC%2F6Qc%2BAtKf3%2BRKnejvRht2etBedwa23gHmUxyepglbCl8srYwkHD%2FQuc2U0DJ%2FAW4G0n89%2FPz9CAe0jyfQJn7nkkRVlOidNYSqLWf06Ue9jPMe7FnKLzWCmbAwnDLjcY7WzjpqjT3eR75phV%2FgwF6roNhQFJl8TsIrZxa%2BTXc4tI4FDuVU6nrcHOY6sTeovOMXoooP0DhUkVKuPbWt5aFHp46%2BficF4r5gy96qi6O7ip4EmV5MPIsYdFStGsLFuAgap06QUUrauHZ18iRd4HN5WU8PmuoxpQ7sgZxBFEurCB61rthOgzzBF%2BFYc0tsnLqwWxQ3rC%2B7U%2BGtdDe%2FWq0HdgkDxGZY2APOd28hs6ZYYoysSki9ecgeOer1ytkhQ%2BJP%2FOG3GK2tKn59zD3x5HIBjqkAQeao9WJhrlNCrlmzmpBqExJLE1FHI5c6RWdPmh1AK3SDkv93f7DnFSbV4faxENSNfs8b52dgKXexgdMkBeKHq%2FXElrJpxwpXOKKSjfNS2SFbcyELxJNs47RRLSvsBKmzm%2Bb60gYzSp%2FeK5wwrlh5K3OO6WoPmffvjIYwHq3iSZn2d80%2BLJaF30gvOT3xdylv1s5DR3xS2LcJzyl%2B21PDlHE4TNA&X-Amz-Signature=378597bb24719efe96bd5f48d3bf622d8df8c958520d27c0041225f02060c5bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

