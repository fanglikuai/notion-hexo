---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HAGBH7H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpYciofUUsq3cklprJ68PHmhzn5Q%2FRIs3l%2FZ8drJloWAiBZfgabvuOIDx0OreVWlkzulaHQQeMij76TzYlES8Jh2Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMVzamFRnL0A334MNbKtwDVWs8NAGYRG3FUKMjEja8dPT6ArJqO9VsPh1re1CfzKA8SRuwhuJKbY4cxDMhSZkfikPGs%2FGtRPQH%2Ff9H9MxvRjjVNt9H57dFfSMWDlzFxQOi%2F2m43OByNoGigvxfX%2BlLRC32znRpgTRvOs7sJzPEydx8M%2Fijn%2BWUlHb8LiFZZF34Uls4Xa5KcBAfVXHjp3rQjRqqYvmXxbI8yawfGUrY5wwuPQzMKnzToRtQdqlImNt02s71yONWcrb69CDrymiYbazhRQIAIok4GBgZmJyRyvos3TJ7JN%2FXQvuDlPw7fkvz2GFIQbBd%2BX8mO%2Bs1A3sBZbvdLnyemxlADdb9gmwM7J6hwpEDWiyRRr7l6mlhQae5aH0tkDR98OD3FPRt4By0R2eT6%2BRw%2BzYpw1D9LNtzMlxK6Fn9evVRpl1uUjxCvxPpuIDgf8Zh5wlcqrDYFzMNIL7xa%2BqcyAExhlqTzwvr38rFgCdSd%2BqPtBML0h0JVMc8mxUz%2Bl3INDzMr%2BCw1u%2FPcK4FGoD7gQDJPdBiV4RKOvwZ1H5NAt3aNzV5re12Y6Pre2gEUNB9he%2FJJXfNrpTbmmLiD%2FnuhomwoWwRm2FOcrQIR82UeW%2FvoniL7HGkJW9UxArWH0iyORU73FMwhIKIxwY6pgFbfO2oFBvLXD%2Fv%2BWha5NPPZAj5UFzZEPSSBcPmKF37B4%2FxEyXn%2Bdb%2F9500YGlPoDIf%2BnfAlqNItQ8R5%2B%2FBpVqiL5u9R17hXHXGTkbb3aWwjcE%2Bwk%2B7P9P%2FKBugyxNFeCWISpnT6WnNLtZ7Fsuk6wNuS1Z9qZK1szCo141hJLzOrZCRa1TC9Mr6%2Be21ZuFvHRdmvHZbrZUiAk3bwRIfLo4w%2BHnFNfyx&X-Amz-Signature=abb7498d9430ea81d9c8b33ae92824bb3afa20aa4fa9dd21e06830c738e75cb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

