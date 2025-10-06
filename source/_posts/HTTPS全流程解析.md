---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QCJQBWM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSoEbi3tBGg%2BdckLTt5Wc7zFJIq3eBe9pCfgCK9L4gSAIgHA0F0yc8r%2F%2FSG%2B5eBD6Ubvy%2BZRLcEaTe5VbttKRYXyAqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQlli%2FII11nKUptJSrcA3kr9mGMqDR5lkfFfUV6EcrL0Q5ticHB5%2FbwEyrQJIqk5KvALapevq6nnh3hYrMCSE1cUlQuUc1Qe6zuW8E5N5yGTYp9b2bCJShM2sSpGo%2FnyjvpRIzZN9LGH%2F0jZgebFlI2BFlmqZl1tbiCPW%2Fb63F4V%2BxdsG75bfAamwuzboa1wh55ynFpJ%2BaXiLnzZPL1r0r0tNe7gd2c0oqRuEC1u9QKnYPhS195KNlm4OQDzKFVWSpxSksKD%2F9%2FVZzXUVphyxDZQnsMa6hkdAKHaHXYMc7Mcz6dRJSjuYMuzAZ4A9WGY76zrVU%2BbnkDM%2FVaW5h0Zry%2FsPciQuWYKUDL6nvIKIvT3vabec%2FhkLhvuxjFfQcqQbwk%2Fbk3fY%2BCs0BXHjOcrDABVyhKItk3GgNEkd4ZyNY4U%2BzaLMK%2BfjUR73iCDEb%2BnoVRjck34OmzJZ2vgUHVu5t2aYNZPkQeD512MTKcZTkwsmB5zerY05oV8RVWWQzcCLxLJlqv27581CpecNXIx7HG6xoCpvS84YTnheaGMfYYnftn4nz0FquDg4X44NuKW8KZZ3WhJmtg7rt%2FcgiFU%2F8rhFGk6Vrj3SU8wpz1LEor%2F2GuUIDMyOq72VDjrh51Q2akHI66QQpJCjEfMMn1kMcGOqUBD8O2mNYY2vMfX0baTduRzO3PWBxcfuNXKgyVXiIsOjt89RiRldVxBuLJ2wUspl8dwKWh3p21Gy4oZKKrysNOTjnRpeTt5ENMor%2BqPyWaCWlEys5N4mBsxGZ7kSV4M%2FpI7zdE1vHPZPmwDdjuBwwYhz4Iq2wvhF6mftlO%2Br4HZ8igkJplrcFmUAV%2BCSFPjMGNDpEVqEVyjzb84T%2Fmmo%2FmkocRh3Q2&X-Amz-Signature=87f60be2fc2665a726ef7c2ef4cdf9fbe4bb5813c1f9c0c57dbf76f9b7687f14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

