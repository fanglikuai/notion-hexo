---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6A6ESP7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNQu4T7sufa14wiLhBugW7vVBLP9iaejwNTkMOc9X15QIgVWSaBylFLrFu%2Fj9k7YiTjzvAJ9B%2Fx0h%2FY2UHmIUFYAAq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDO9KhYTTPNexRPG3ZCrcA3Og1UJUw6sbUoHxVIy6FABVwvjRoQ6spa5c38za%2Bvy7mYms8%2FO3pj%2BW1u8GsHokRwWhZnFd8LH1sgYygdomFJptdSoEMGC8TWeBaDnkQS7fxBfpechBhij%2Bf%2B3sLf4E2zA36%2BKcF%2Bw9gWJ7PQLK8spmBsiBBFV2U6qhiWY8FCoxAHsVl6%2BbCD%2FCaY9mPLuW5CYncHO6jKnPTP66VJvBzdKxb40%2FvEAqiUwnyjlNDIVr5mSiUBwovNStC%2F6XS6kgbqPLU6JzV9CTUyyFij1AIkSjkLgBiPBz94CTq95SrKIzDzLHivhuIfXXtponcIkF%2FAUhehpwc%2FEjUqdLgIIBSFCNpDoN0m3L02mAlJGRetJcxaLRUqWJ%2BGdQk0z6F3wOOGSiENtJ8AFS7lMqwHoQsxjo9GqNQwuPalMm6EibnsOVBx9ySW896A8bvD74vSIqOSr7Iwa8IJGWf5uoCf2zJCh4MAqxefvJjIdyqIwJeemUwhki6eEyrrwE5Sur6WHA4hpju%2FCA2HGQ6CU39%2FbKle%2BB5Es%2FIrGCfIiTTI2J7N%2FKvx1nc5pScF%2BxYJMEQyaR0Orbww4diTgGXv78fd5h77gMKc83rLwazAjKJRg66e7rGvy1Oi6mh9NkRw0BMKGNgccGOqUBdGMuwHOhnmi%2Fq8mrY7u3JzmS%2B%2FjgTpF5fiD46%2FVEypCHsT%2B%2FLT5q1qRusAhuDRynKhnoyZNGB0Uc%2FyNNOENtGmGdwLBpe4hsADQ%2Fm59lsyB9zuaQG0lB3azpE5f75Ul2p4SzivK0yi39PcgOPk6BUxllPO0dn2OFpEOdBZj8%2BIemQu%2BGavYS4drNmjVdq%2BkfryiGjPdOa%2B0VMR8ethyfHPr6MOI7&X-Amz-Signature=3d9e4ed64f6e7cb862858622d9df75a085084696579fe11acb48ba6bfde39e47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

