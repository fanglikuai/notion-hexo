---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VRDHPBS%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5s%2BKR9babMCwiBo5%2B3Unuja%2BmmejuxgqQpPotNiaLOAiBuzhYRe8lYrfkRtWofMX2cFgzAZQLtCYb8mwQKNzewrir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMQ8jVhospGpE7UbIkKtwDEuVUn7zL3djXC2owQ97xNwZeGF2mVPZr4XCMKr00Pl124hpovg9s5v7Jr81yeFfjH%2BNUmGEhjjWuLBWEOBU3iipB%2BIbG9uIPT6jQc9bgKbmWunCM0ovNukpdn8S63H6dMDaOgc6%2Fypop9pipQ%2FYUJTriYoBuWNlSQZJf5Jtd0yx35IHJWaAuwtf4%2BFKFCoMJOSCxFJ1VuGHV45h38axRRGn9SfQ2V7bagI%2BwZ87FtkTUDcUBP4HnBBy6fVAvJHyFKN4xTN5V0ISKpXX0olmTVif%2BOG3OPz4ixJsv%2FuEa3Esgi4agxbUzz4R47%2BRtBwYkc7xzLG6sItuXTLRwVM0FAna54dXm5yHqSdLJclagJcNRb%2FRFw%2FUceeJI7tYNklXpGmN42dAh%2F6OzHD7NoEWz7t%2FYKBgVEIAbkMGFP7BX8s2aKaQW5yczrix6SnZ0WZLJitq%2B2KC%2F8ban6r6hc3hsODn6xlXMSmkcfAMQ%2FsjHWNed5Fn7L0uORU3ztR20nGiPQijc2rB3kVReVlpDGcIioauLVXhJji5muQlYX5FNqUGIqJZvk63kmEXjPQSK87ZM7vt7LaS3FrcmRBVBSYTILc0Gh7S%2BlqHYLbfTVXcR2PhhRNVnXyfuzAKSxqYwtrq3xwY6pgHI89AhZNQxn8ZXQ4M60hjuI1LhmjJ7nmSwbOUf5whoKHtojQ6bLrRyJ%2BQj9IyIDnux2qsdHUwBAS%2BJgC%2BIuyzMw%2FgzeqJGp9r3Rilr88V3OS2smDQcv1rBp5t3C8vKXS9gv1c8Dw6mvabM3jYLZGeI6vF%2BcNs3QV%2FWu6QYCmpjGPB8A7vp3qZoEgtgQD00nSeoAhYQO01iWK7elUaKDQk8LDvst19s&X-Amz-Signature=90ee668e123544ce3a57b03fca93b8c4ce5a4e0ccbb22d242392c54fc2b1fbb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

