---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ID4NYYX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICRGrzdtRQ3cuVeR9kB1e2XCe00rCCEmCP4BQ4vzQr93AiA%2BSVyzSTFfzn%2B0yFTt6WfPF0LS2p82AiE00m%2ByV737YSr%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMsgwg%2BVObsq4Whbz7KtwDZMbzCXgs30N2wbEuw%2B2oXnpmdxwFl%2FSEuO1Iwh12q42TSY7M6vImv36OxW39MRwpPNc4nr21ScAwYSbhzMMy5VxGPQ7SeLZ%2Br4r8H9N4%2BehxH7t6Jnta2pS6N5JNBW8%2BPYNrd91fDjSjvLzKzUwsiHMr4GofxeGhx94VRvWYU3BV4DY%2FE9zlVFsHp1fgLi71Am44BZA6pPcr1x7lVFcULdzp4LsCCMccI6t%2FYvvbj0Emd8XJeacJzqWzOPGIzb%2B%2BusteucvVFqI6MpsEJF%2FEpTw3SZlo%2FVXB5urC73Zot7IEeDYMLFaMLlZl6nt25Gr7p58zxPV06aeRIQEjNlZviurwlsP4r%2FG4iMPItmCFYqL5UAA0jLhm5zx%2B86kVV6Ed46rZ8%2FTiIBjxpirsrGIjQLNGdtyV%2FaaqDVBjGPTraXuwfZduQMBCIJ1LzIIVapJhsWKj3qr4N1%2BvdYvC5zkZEmwcyCN2TBp7NqoHqx5NHB2cHiNj8UHt3B2nWuSsZbcNCe6k7w9EISr%2BpESB%2FrClK%2BwwAEGdVsZtoI16ujmxz%2BDRUL%2FOrmIkN0JvI4E0Os%2BEMFbKlY9mdhjtskBcSxTkeQHp%2Fm5cOXgb13eEZimANyl2RRfQh%2FgngozX9p4wgdTGxgY6pgGo5iprjS7wzrrNABXneumMtH15snED3HbJ5D18PhbzXXS272yKJCDZifx3gRIyzJ2P%2FSqSIbR5%2B1ICUCJClHQ7MsePDlu5WjkVGTSjv3C4FQc6PBWNmGQztT0g5ZKmEFxxDWDFzyg%2FBCKUZcz4jOcdBjaWdEXkOBgYrOPwpc9P2%2F623r86qBXr9oV8AEK3jWZ5YfsEcvRvPoTISeFtQI0WXolqNBQ2&X-Amz-Signature=a7862b087d033ba2ed0413e121a9ddd0d78c21fb4ee539a650e22ef48a92cce8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

