---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTZI2U5Z%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQc3dXl1qo%2Byij7hFBcJ7JrK54vENQeknUBD8AtZqPxgIhAKnFcGDon43SSum3rdhIIzVjq4I5O%2BlybestnPqfuHQFKogECKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysn0Kq6HFMZe1eWM8q3AMcyOZoG8m%2BpEsYCIIHsRZmEVTrpze6oFeDluIJ%2B2msJ02V5CfwEpqJ2xa9gQKc1AZn%2BIpG8Kmu%2FQWsuVd0gQKloxESyCeqQ171CljmpPH5wtjUcrCoHkUim%2FiVaH2PHZjWFrkFxhXrCCu0z6pyyY6kMSS5EKT2flZYUPW3zx2z7mfYSwePRR1PcfC8rHmyOezRoXkQ91vqvfS2%2FLWfuJTrs%2FITWmumC6eIAmioVlZCkFshlpr7eHvXCmKXBDs%2FjtHy59BXUpkwR5Vy7ExZu5gikhwNINn241T6KZfrzDh%2F7AhzCdWOsfW9tTzmSs%2FcbrIF7qIoY3WyJIyEeIYrqaeE%2B6ZXyi9tASNHi50RX7FOCstWaqhTAGS%2FUjS%2FN74v7xK%2Bbndn4gcxzAtnnXoVF%2BJUDsZWUTW8QfrtCJAzKlrthfkeXGc9O3k5BLC%2B0oK4WZ8sn4stlTlsr29wS0xD%2BW9xhBf%2Blts9QRlED6wZatDrTFwKqVfJ23YGUGsez46hkld4kyYB0EuQE3bQOIOBzMDp9MVS91%2FMXyhSYhUUFoOJLUflTw4gaGeYaa01ldf0sgizm1BSUuhDTTv7eK3xC9dz5xR9ru2i6vL6L6DA4QgVoOoPD8vfHwifNIvzIzC2ov7HBjqkAe7H2g51VpZEXEK9kU3u1ldlCkuEt9RSVCAxV%2Bh%2BYSCJMYGKE8pTadB6aiznor4i3ABubMdhY2%2BLs2iD7r%2BfjbB7GuNquiDUdT9x4jsXiYv27IZtQwpoMyrka2D7Rv2XO7TPePId0lT8WUL9n0CcwW3gqAFRSojTiyQnMyUBEeSL5uwK0A3%2FftOOQ2KHo530Xyz0SbbaI5G5Jx%2F30rRUuYIkUR4c&X-Amz-Signature=ac5f5f45610b73de20cfa7f5f7fb691bd5a75aab48314ce448bbe58cc66c2bc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

