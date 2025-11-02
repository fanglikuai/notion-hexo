---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZERJOFLO%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDzz5Vye8ywz49iOT0TMXb6N0kswqA3IdpvZzII3u6cnwIhALKaUYxrhe9sLhtLBtIkWY%2B%2BuHPn9VaYV7%2FOm8rXSumDKv8DCDsQABoMNjM3NDIzMTgzODA1IgwcPhiH3WUg84KxGbQq3AMjdwGCHOlWHMLuEayzXVp6bY%2BdRJaq1%2Bk7XYmXZoguCYMmMX4tneP6CI795NQnn2U6wZb7lxKbrX6MtbcYqyl8jBeR7FO9O6ch2sYiar6f9cWw1WhOMFlknZerEj4Dy9YDHZ48du4xt3ktzN1qqA9IxrfPQ1hZEPJDa1j8%2FtyLe9ZanOK4%2FSbWkFPGV7Sb%2B%2FpE9HPQB%2B%2BCNZ2TfEw2mdYBBV9ZECBWwx5971HJR8rqSDIemH5ufV5Rt6tiNByHrGSEqB8sv%2FwOd%2F0%2FhzMdkvHAY8vhBm08uoe3gyLsYyoUuc4OiGdxPiWAzw%2FFDaG4xgEjLPabEwlbUb58%2BmUTkjgzGfdj06LIkX38pVFuLHMCj6WMTDmnUzvbz6yJOFQ7q5uws%2FLlkZRvitkG8wE10DgYYUHmAwxX2azQOxsZvpdOmu4UddQ4fFqid99s%2FxP5KPDuDiz4Rotie5vy%2Bgye9H%2BU1%2B2N9Hk%2B7%2Fm4rIfB753X7dleCLSneIIj5dxroguVMJoEQH5aR4559CHEOu713xJtwEU4E6KNmuDlqvIU7Y4Z5zwZSAVfHiPi6bXQdje%2BogVtg5xtMTO4JPE8kHMqvAyQ5ltEUv5iy15lDUTOjyrPyE99JYd0PTQ98VchGDDG8JrIBjqkAQ5QiDS%2FOOliOn5pPh5H4bqmB6QATfNRfBI0iPSQXLBQfNdVizq3ilvZvZdBkBbzwGpbe4zJ931c1LtVltRVmihvxYMurYFLwzB5mQEEEr3XwWcBSpO%2BEWsNJ2O5saHDbTUGF%2BNmfQbuYWLkstllr8HX2wWjZFQ9THkiFfYyxtKLW9KkbVa2hE5HnVAq97PPbB8vTo5NJrIU8rKicjMpdZCruqsL&X-Amz-Signature=d76b812df4c19e0f5932aff1ac8ce26c6576199aeee93124f1be36520d1015c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

