---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNPCC74A%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIFpqWx1QMX%2FKxfPE8msVzzgfREExEQZ8QLE7Q8hM8DR8AiEA8FuAHjO7u1P0UML%2FgRWthNZ63lPm2n7W3xvC7LgTeYMq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDATW8e2Xj59h7zk4nircA2Yub6%2FDeww1MvwRAxxJvGRF%2F6hD8aR%2F9qBPYxyl%2FxAv2cHgYiubGXy1mfLG5xq97lafqC88KSfmI3ailg%2FJ2cjCzJoEEp3BT8bjb6SF2WU6eta5f7CL4cIu8OPxTEU59Tp9wgWe8Wv%2B8%2FFufLAiVq2L%2FZkFQ%2B3R963kpnCamJGj01%2F6xb8sZwH8EqnH5MRGgB93uAmyCmjUeVAfBUrxJShUQRTgl1qi7z5LXY52Z5upM08zG9qN8C%2F8k32d6fQiTToSrj07bbkOosp4j3jbL88rRMF9WNlOUVRoCSnuJAc1df2LXy9R0fiLAzmtb%2B33SdO%2FQSqsNFCEFnckw%2Bsb49us9TYH0QXC4EKmimKAXeBkYmXXk8gd3cX9oHwc6rl3unGvd0lFnPlCzGQBFaDIFeiZJLPUaP7AjZvDo46Ody2p6RP9q6S%2FDs7J5yIRyT8%2FYZQdCG4VPZrMj%2Fh1i3sdI9mcWHFkRv%2BHZbr%2FURJ%2BdAcfVQvtj6IbfEVHgf1fgCrV8CaNo9pE8ysaw%2FT0%2FVyU5aPZpCEay3ARs612itmwruM71gSOViejV0AqlhwW9CN68A8iK5DR7erzPYAe2u0jR7MBChfR1rAmreAl9mNsV%2BQwpgvqguA3Li6YncGHMP2YxsgGOqUBareyLUKrY1JR2%2BKL5%2FEbSnNGsnma8DKl3%2FHX1wR6471epWpQmxDLDa5v68YxDXZmeIs0S0Q%2Fl5Gewn8wkLh2JqD9mvWn0YXzfBSZl5%2FSi%2BX5YF9l6XwA9B8hOE%2FQDkE9nj5j1Nun5S9L5C03aPffPmwAzX%2FG5NBKX8lKqWz80HDV%2B3HFKzC19aGA5RLG3Gk%2Fd9g51w8Zwl2LwIz8Ju3W5YbyDo3o&X-Amz-Signature=718320617a130ac80ddebb8c3a58be3254226fd3058577d4b7d5dbfd661255d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

