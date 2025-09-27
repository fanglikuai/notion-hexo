---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652BJAMLC%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIDmuPn1sM3SOAb%2FQLPBH9CSC09hBl9xIUQRHt3GzGlMwAiEAijIOXxA1RI1cEkMCxPSW7Pf0x7bOJ%2FMRsNj2pee8cO4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETe%2Fvzr62RsIk2YzSrcAyUegv3ujh67lborUQDEXYOCKyUGASCAaa50wTnWgIachFSFQD3nnL2Mf4jwzPlVe6NugyhGUwfesrZGqUV90U%2BYANSzjKiggq2Bg14Lrdhhz8hvXulBg3hMqSQBRnh3KvK5iwS51nzEeEU3Gr2p7FKWAtqwUzpLeK%2FD8SLTKxm%2FK10R0hmrDvZENMutzLvntqr8LLMS6NyxxsGl8HscI1pC9EE4kQaxAu14My3xKs9kofWLJ8NjiqQPWgAxOAXaaf649URZM1n%2BkSZx3%2FafECo4N%2Bzutg2Y38YgmX0h0a4s%2FDgQjDGxsk7FqdTQlC855i1QEZiA7dOm7L3hB4%2FN6FOG6A1vwqH6oAfhMrnZRAIjgql6%2Bl3htm5KOHa2VnWgb6PqDt7Mws8ul0PkU85MtJLcgQEcbLaI0AdO427pN0KcOiAIk9G9qB2BTfQpwtpFyxvl1HQLkrkm%2BYwrFqcCil%2FYQZLx4x%2FHnf%2BWxQlW6ZWxk%2BpIjOM2lyMmFkzB19NtVaGZ3xZPKK6EXzeKNP7LTdNzZImHbONQ%2Bbg2o8ALR9w916SlJbny5R7YGpq3b1G1uM96fKf1RdOLFFbzRxh5y1gIKrzuUK2pwQKdfsnEdEUf5ypAu19HXXOhS83kMOXc3cYGOqUBpilb0eoCflWTbxwtq4Rbx4DylMPfA5T14GgGg1x7CkXcpJY5UZsJfOn2KKjMcvcFeCbRU9aIB7JaPuw7Kzf0SXISJLV8DUYblVc5M%2BG%2Ffr8wp%2BgqjPVFuoRJJM5WJYKyh8R3535FdpaGLS61JCFbyg28T2gxJ%2BtMuqgfB44Y7ucJY2FbHKJ2ihMwdsPTiuYQoPshSoFyfed6AxdLGgsNXD5ZMoDk&X-Amz-Signature=9b744a63bfbc8482a5b3e5059608d4c5c38558af79a396dc34b6fa234bb5dd07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

