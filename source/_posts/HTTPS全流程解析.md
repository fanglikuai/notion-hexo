---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PS6HS4N%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDi6%2F0r7VjX5%2FXGFCO7JrSaEBtV05Nrg7rxfj0bxRMUTAIgUkRiZztXuOJsbF8olXiZxZk2odjzg7aLx4tpR9j%2BoeEq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDBxQ2TfBfx6Ay4Vx6ircAyKZqUiOvr42Vm7EfJLSpOh1cxe8oHKkInefoOcX3B6mNGM1GuvMuAySkhTucpaeQogJfwlSLvesfJ7SM%2BG9cwae5huS6pWyZb29%2BSkHr%2FYAaDlhJW0KYFjgB%2Br02RnCc9Dl%2Fma2KAgQ5b9Et7cN2vy9cEqRjt6XStlHE7pmMgOhjkVIlHt44isP7HNoCLUkRnmGWzuHmlQ4c%2B17JyrrK1Gga6vmyYixxR0SdV4Xa2RY74CFOMioU7Xe4okQ6ll%2F%2B%2Ffg%2FS87NGiOE%2BAmNOiagzwybinxt9OS10FwDmXvGL7QbUyjQC66ufeCDXrWVu%2FYzH83h8eKjjmTCUxbg%2BTF3s8CnHYAl3WXFKfq8LI1duaPczjvygoGReXXtnNk86vuqqRE03mrY4bdlvyOF%2B6j5AHHvP3A1IQrEG4sZXbw2K7guGoppENSP4%2B5j0Izl0F%2FbkJYsS62ki0DWWJOgzyafzuJWrhqL4crO6ONfxv73QwB2z%2FgFKr6F2lTgRS91ff75NLH48pLCYSAgOiRAs5br6kAe1BTV4XCbKt4d2G7ca%2FPtmB%2FFIL%2BljZvv5zXx4TLolKCJ%2FeykMF7mCJ0CK8GrGDxtXrZEFe%2B8PgxSI3WjKvLsdyokavHLNT2kwjSMPzOs8cGOqUBCzYqQ%2BrdSljzamLvDuAN7YB1qpFETgKZWx09NQDenSihB7Cup4P8y5pWImozoWV22D5LNmzL1lkNt5f8QbNqdHN0Pt79863ZI61OLCQnb8xvljLnalvbb8D7NrEMIn1P5aoOEPstR6iAbflt5lcgyOzB%2BY7ES4401jSUAJ40gW2B0X2P%2FAZuYiOzb9bTj3K2O87QVrBe80KTTzZrOZq2DtRdW7KZ&X-Amz-Signature=d9f6db03ce7d6d000dff2a3fc1a37085b90fc56a5e6d132e6e6e3b75ea0ac2b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

