---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UTA5CV7%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T210051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFM2o2LFXeg2b1mHl2h%2B8p3vWDNgePMOusTLAstD1UctAiEAnePFRL%2BOMSV3r54lI5KkaCPAllHYRI4BW6j1HIpJF6IqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGy0%2FTt2XK05ZrjCsyrcA1JMKTn8l9eCdqUJ8AADFNblQ3THRRmm3mPKNyKSmUIIxDxzaTGCG5Fr%2FjDyPS3C6CHZ5q%2B98smPaHqoUKjzPqo3sdcjM3CcHF892WlpMb1TL%2FEkexCpX%2FR1bVZ%2B0AuuPXow32F1qbG42FGLd1UaYxnnelZP%2Fvbuv3xxC3C%2By8AojC1jjZw0KiNb%2B%2BznaQB1v%2F3mdvUrCIzhv23CE8f1aYdJFHwIWvLP6OGKT3AOHfRM7J%2FLM1wNLbLOM%2FNQZ6H5VBeMMmyaUjhuCy9mcKfM9IgQ13BfstjYJ6qJOxWPvBQA2zPAPmvOHl2uIb3ZsI0miw%2BMzvkMGV23cFk3s9h9zMxkpiHoBWZZbO7xqJ9vHLNUltzfRqEAmBD3%2FXH%2BU0I%2B83YORX%2B%2FXnOGsiFV1YIufNfUhH6wrQO3CZ0vP%2FKQps90VGAp21zA94YFJDn9eWu2Z%2BdinPALjUou29ZkNRquCOGpIVYvy5PXppX6iiJrnp6jYja4vDur9HO%2FfzlLApCYYyrXnL5JC2fiE4RefCKZPRCG68r%2BtcMxKwtEbBGF36qoRzWypfqePB9TZ3xUCRTGOTEAeThwz7190qSZ7OneFQmruFvphtfI7c9oa6ykeaQWPRALhYV8uR2ahezQMLq4xccGOqUBvIKLb5RrsZJ%2BZGRNCvtrKUPbLo4nRHOITAEIWtNjlbQqkEp%2BsXPCkbhRnCbzEGzMTyYwV9f4%2FdeduXk%2Bg6GDcToP%2BjxNnmgaMHsrIK917oKuxqvjm99puEoNsCRecS%2FupHFFn%2FffTs4Mime3N3K0iT66ZYgLclFx%2Fun9lI5%2FtDdq2Pz64u%2BoEQ7c7oWJtUUkkLDIoaRmxw8%2FXoosDr9sNjSoS1iG&X-Amz-Signature=c499cf66101bba1d1ab2b7f4655d420ada45c7bf683e4a4b9df0c9c48f3a9a3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

