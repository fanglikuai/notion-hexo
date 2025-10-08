---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WSBZ6E2%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIEny1jsBG87kOYSO3GSyvY1ce1CiS94IWwMwy2u7pMyDAiEA4jDAMOrT6zcKCz3h5TdAiReBD8ZSAS4qIGPgVLsvk4EqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOm8PqUZnc7uXMQYbSrcAw3SmhzpdcO4rNH1wv%2Fz0x1yYiJ%2FRm1x9SyToQplO4rHq8spAxh%2FGyTcrfrPZPZRa9d9qYKkC8e8TuXVmZeThM7lZhwSg%2B9YN3HVo2JUg16wGu%2Fv1K%2F70hIrm6XAfDBTB0PfxWexRT1YcsNTdfpK5tB9IFs%2B82xSYTbGnbj3ILVZ4%2FyASchQ08q0UOLRErKZTYps6TvfI5YZXAU2GUlX833vdck5A9xdvGJ4KOIMga8JW31NB7WcJIFnMj%2FujRtQVaaX6QWmxnhr1jwXnDqjp%2BD4R6m4vhbClW%2F5qv318aFjgytai5q9vKBbTP7snjEouYNPPQEw8sbz9hD1HIuqcKA1t88CccTwleChirNrpp0W30pti2yuVGtr43h8SjbdQg2fwe0Jj453jeYXkNt8x4bxc%2FijUGAEOelzbKTns5BDVC9CfobU5Mu%2FhXvzWNT7u6wXYeBEImHRlUfTe0NuAeNWthmEVsLtSXjsc98%2BqHeu1OxTybBQjktuf7H%2BeHLdEzsy7OBGQCTt5oby4YNcD9KPAc2yQZn5DZC2RSbLlNSfQE4wB9niu8gx7%2BnWHwE6QNF7fkNe7bqO0N6nWI5sfwBF4%2F4zZqOkTJ2jTtBzAx15MnIYw2phncwtvozGMKPOl8cGOqUB3Hp%2BXTmv2ux1%2FJHs1YDdSEBI1f%2FHr8ti9RZf%2FcEIAeGnAE119l1Ar1Irku2Ww2NRPoHR8HWc6KCNErXaEVJD9cQ%2BvxxEFiZrg5mpCODSwa9IS8wjo0D37tc9iLdkLEnGI3K5Dz9IMFp%2FbTM9LHiMQ6HM9CP%2FmqMlkwHSCU%2B%2FTBSq9WmUybmWRP5uuH2Zbfpj1w4v6Z0YcE73QGsnDU26OghUMRJf&X-Amz-Signature=d08a49fbbafb0c1d4b8349d615449abdbadca1849f3ecaddf808ba098fbfc8a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

