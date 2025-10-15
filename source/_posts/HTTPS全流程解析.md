---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HWITDHK%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA8cutE8BAo%2BtX4Jf0KCkVB5BUYe1Nwt%2FaWfldOWVbv3AiEA27Kf9ewKYpHQoVzl6H1QHD9zwB%2BzUcDPU%2BWGxiSS45Yq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDLKsy0sFQP%2B30oXguCrcAw9A4YzyfiW3Rsj7Iszqn4lgXjksan0ORJZthODpjvPWJEUxFl2Q7SVbWmwa%2BbGQdjIOxW9psV0zb%2BXo0bndLy16WyzzIN6KMXW%2FB44M%2FBf20jvX07tTLXUjlUn0IHu5%2BVDQbvRsVAbYyQqu0F1xG0FnIZVqpcq69iiRvciIU5NFoR0KfPoZxcSJfijaIGCF3Vp2kZYYFxzo1BEhXpzVxHnT1CsXEe6SREIPqJsWq7radqLWf6ZFIaskmXJWGJyJqpPUJJXv90rPBcwJ1WmZK%2BPvQRxJRThnKwJzXHNLzho2WlDIRCaG6ntoKiansaLH1GUH0qDWbdeZziq4ZlWR3QR7DuXSzR5BnGHPzBwovafGv0FP5SnjPQzmTgJDRN56bl7tJRbqTZ2ZOPqqNdR3EycG6Fhc3ZNFnAgYi4DXT3WYK9Y67cXG0T3Fs2eoZHwM6qtvwD2RL%2FpIMfSsXWLQvGqaBqI23yM8QLyAJJhe3NPWaJaoEeAHAuXLhDJsh2WNnQKDI%2BipM%2FzigMptOjDTm6Ja8hU4saujhVSRWB9UIrkSiN6v1Qfi305wZzaouiKSxwbl3yodTmn0C0vcy85gNuFJGt5jTHW2daheKgXaYPEYpi75%2BDI7KUQIxbZ%2FMJLEu8cGOqUBWqe4h0vRKPtrL10ZUntb5TPHK71jhrrO3UUJ6GWGkRvIwL5%2FSWKbKs5efCIc3WFA6wfoeubhWva8gxbceasv4%2BKNX%2B7AL9aKLy%2B4tq%2FdYNijypC%2FgRgeFzw79b2LMkx2oc8foVXQmDZJhF9JlxbLh8OX8d4ChiAUGwv304EHkoDoNDP%2BqYUh4cchTrSJsKhX7sXNr7FdJ3quFvE128%2F2FmbeRH9Z&X-Amz-Signature=de7674fd65701f5157a6da906e7dd0478684e35e00d6011757146ea5a8c3a846&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

