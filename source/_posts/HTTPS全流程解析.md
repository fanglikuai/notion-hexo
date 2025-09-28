---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVPLTD6K%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQC2ASy%2F%2BWIxgsBF3xIiXKL2aduNQeWkB3pdXdESKka4ngIhAJM7BdhfJhqX5WsD90Z%2BGS2B5fJ%2Fe6me0WpLFr6aQa4CKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwa9Yck0piaLHHpXPIq3AMWISM4OgOvQ%2B%2BozwmPKjbUAMKB2tUHjD%2BVqHwU4XZzoz3NWNt0oBZjB8iH6yudFzUyLpZS%2BlLNC3mMUlVa9NMz5b7e2yCbDzFaAoFgPPERlHxJfjHUDH6%2BhusNGHJbUaDEPvQ%2FlQN1a1xK%2Fn%2Fid8COgP5CFcBNTSp2zGEXboCSYMi%2FxpE1ySyNu81LDykThm%2FBm2pCcwMTQ4sXn1jvoxJ8Accsqg2s0mcDK5lXYJWeqstUDZr8w%2BgQmDtBysQrjAf%2BcPV2G9MRieuamq30QVz2Vy0Crvs%2BX9AlgXqp5PRH1uL3sF5lkjSZpaftDJqDrYBI9kMwCxUCvdRVTl0D1hPTfClqzxYjH6Q7sV5Xfsvksz81Uq0VEQoG0H1anCHKGx6lriwPUOirXp4gMAgJlVB8DkyJ%2Fe4WGLB0JMNpAOzecKqnAvJrrdE8sQUWJTIP4zQsbQee2YwVOkO8MD%2FuzEVMhKzY9iGLy3Z3fdZkeB824jK7VApBXunzqcK4D35HWd2pfXX7I%2FHEG9OjM3M4FW4twT2LmHqCLjAsIsVxCDNX7cFGRJAbNu7U1wntsI5oMH6u317oIsyTXsymvvdcpiL5DdndwgDXd2H0OSwdUa81sV4%2Bm2%2BPCEUxGILTTzDwqeHGBjqkAYt4IbyHPiYmbRegYjiTgJqPrKWmJGa05K%2Bj5n0%2BYUJxEguFp0qQmC%2FT9B%2B8JQQaoRyQLUgpnm5Wpl%2BQxNI6aMihlCB7NXduA%2Fb8nqdMAd9YCcZj8Te1oafRRwCFUmaHQS5NeLrHSN8TpXBzxIkWGP93ZyXFD%2FwpgG9q8P%2BYDTPhaxNIFIBfhmnQ6JD8QJSXQNgAptuYya3JpjeFhwHg%2BkonaT2v&X-Amz-Signature=fa59102d892c70e544299244590c9f41359ac533527da024efe5770d1643de5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

