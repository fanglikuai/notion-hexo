---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQXKU3J2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDva5bAS7mEfg90nHdR47SJIhYzkoSl%2F1Q0eTwg%2FyMFMQIhAIDlrEV602NWNk9NT31IkXpLcP2UxAVHV70h1X%2BYQyiaKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw3I3QPehISJsns%2Bh0q3AMVzb0n0NPO6ldTe4s71Hlwdz3fF8CuU2cddbGfnMoVA6M%2BrXqN0Rl%2BzTnBWmsmGgPeaSChFmvQCD7zwTWsjgTUTFJLAGLSJKWKLSyOvFN1y%2F%2FqtWScHVK7dxBDaCUK0HhgLWClQUvJ1Qqfs0sWmKfTznl0vcLJfYV%2BEWQi871EIWuaZdgzxUu%2BYtYuRES07MPz7LefkFX1lSkDfOhQ3yBwr1P8BY4dksLXRnv409M5ke1H6869Nn2gWAlMEBRCFXt4A0hwqKfbc%2F4ECi4iRvVpfXUjLMMRRqt31AwVlhGAX1G9WUmXzZIZzpn3ySMoXR2BZBMYifcUC0Dmx7NCLUoanmDmVJMKbJxkjS0vZmg0aprylQmwBOwA%2Fj6w%2Bv4bnjAoPN%2BarpJ6z9G%2B4YJ%2BW6gj%2BoqukY1OkFpwgqGeByRShksl5JYimXl3oHRMSlJWDhlVLCLN4Pcj0WjurAcxuHJw5ubxeVCvp18eWH51Pm0vDN%2F2Gb2TYQ0L5MCGvaweWJUI9MW%2F1YDSsP9npuAIIYI2z9i9YST023ZvCljcpMGk5KvZo1va%2FnM0fq9cl7Qx0eYI1FvLPM87cZiHT5Il11N6qZ%2FiB3GLwilK%2FSf7XXLfrLsfaVOvPxxEtlwjzjCyod7GBjqkAYgnwMGds3FKAIWQGKhjqOZfHEtO6UDTOktP%2B1OouVJe%2FER3uVzPb2j5b40fIrHxbU%2FBoN2NLqwVmCQX0m8HJRvJIfd1Qm405ooT45P%2BThdzu9ME3Xv0HR8hjpp2qUAM%2B6dARtSggPzo%2BkaH6bhKsH%2BPBvVVqSvQMKZJTs7qTvygQml5qY5jzCL6vISgDNBP6cWbc99%2B%2BzXl8NvD7mkwVVm5C6NA&X-Amz-Signature=8492a1f742fbdbc31e453dc890178aad8f4d146d9b0f5a4a86b4bd8c58dd8ceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

