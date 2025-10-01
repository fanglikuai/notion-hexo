---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TETYLR3J%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T080044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIQDX7OiQUm%2FK1%2FOFH2PN3QSAd8QKtEL3lbVpGq2PewvPvAIgXFFlWDMt3ITS4cIXi9j6idhaTIfc4CIYYAlAZAldBDAq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHZZx9FwKXFUaGbgYircAzJRp9d1mCgxDp5vKF0QZd9uCCkwMv13vqhW4Uqz04AZgzYOtlf0BC1rKWU8Jb7%2FhDFKAJGUeKbRzuiE03hPHE2tF2HrYJq%2BgOIp6cUEeNffSrqjmB52vhE3pdrUUVS9ft9ILRmsLvIRTgyEaxXWAg%2F0YgiUrCazDw95FVqqF7UHCpHgS5P1cRynUupFUkoAiBTX1V%2B2GdzvPIJ4zMLxYQJkOncGxk42KWJLyytQ3%2BTdTNR6VY0NZQ8kDx8IJi2zhC8XARchUWzVFYY4v2PqSNFRmCh%2F10I2rw%2FsvPRlWvy4%2B7y0Iu8t1K4itngxFzNXrhviYhJ1QqiMDUpeC%2Bz7NQtrSgKdzmjsLAr8uj701AlVGomntfidX9JbFWC4OhBuc%2Ba8Wha%2BXu%2BWqfefRD52IyQIFNn%2BYy%2BdtZRYfLs94L%2FnFjbqBWalYA%2B6FGR29NBthxwuDLMyOajcByW%2FOz7G0FGKM9I7eB4qutnOIeJFUm7dfpKGVGk%2FNBUc%2FBzBCcDpUo1NbdLt2YOBEMAV6bri%2BpwpLEe6z1z1pMLM8me1xox%2F%2FcpGKIH35Z3GfTg4WbPxtxsEjg%2F%2F%2FoscJGcmauPtpKCcWc8Ua6JejU4XqAtcXmzcW7bGcZaf0P1wS%2F9sMJO088YGOqUB470twK7Q8DtEFJ%2FGcJrBwcs681gareqaQ4KN5CBgpMU4KqyqF%2BPWhXvECPTg1xWS9TsjNh5i87pkGZ65XdOtnbYLTsJyy4vahOJx6dKXvYrQ7I3f7rY4X1%2BVtOjkpzbisCS5XR%2ByJMld41%2F%2BRDdjBL4m2gguLVFpn3Y2oxRweZPBOpHd79AcDUaCySWIHDURXTPRpr4yCUMQ3pULNQm7dzEHzEva&X-Amz-Signature=0ff6ea8c611cbd59db26691f58db5a241328b6228c4ca217351088588a5bddc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

