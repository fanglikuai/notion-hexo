---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GAWDISB%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGX981WJGmgaxQizrYeP8AvhMrjVPTGAfHpB3jeEd8eQAiEA8D003GW5Yr1mQpeXPpPaJmLjIuBKxtoOozojGAtNYe4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKaoTaBw6qF1djd8JyrcA%2BkmXkYS%2BvNMnNMli%2BwtCE66zhEFOYG4RoJxZS%2BAbzCuJqITcggMbEAuIAr4ZivYmpBQuGLTtTs1VU1BCrcMTqFJTdzrYRc6gQuRNXeZt%2BWOdb4O%2By3fn2I0a7DnNbcoCXD4CuVzKFJYXPRouo7FKWoMQ33fqGkLdys4%2BXJ44wf%2FQdyXguUmPAcpXtdlneAfw4mI9ScsGlAY1K6al%2FlrykVYKkIg911gblpmiPS0lq%2FTGfxh8HJiSGspwJsogxqOoErpvv4hjdR20UR3%2Btms4CcCSAi9W73%2BQrey4OgJaWHcNgxB1kCNZ1u1qis6VX93uEnpqF%2BJAq0QGjWCIAI6DowDoBZOvpDPJU%2BnfkZPwSZwv1ch0fPDYq1Lw6J3CKIzustxTkO5PamJa6z0dB0a1EcJ0sh8Dll50bcASZ0EIwWgRnit0WG7%2FxcgxHn%2FkVVf2MirUh5HYHz0GkB5nXMV%2BQn7Stl4o4sJ5XLOseCO3U9%2BsSRl5PDBaHYUuHFoTZNy40oumz2MaWhBMNAH8LUtFE4dRRrNkA7TYMYftJliKR3BE0yNl4G002uHW3L4e1Tp%2B2J%2BczJJTqJstmIgSf7Eo9KTRjXDIzAPV50WYffnF%2FBij2wZB%2BKLHX1KFzATMPftpsgGOqUBKUIST%2Fcd7EVCCgbc2Oiiiy0R%2Fu2Y2zMJQNiWNdveDusTP%2FfpvF4Fj9gT9rjyxbtmidub6XWhic60mZnKSAnV05ZitNY766RUdoPh84tCOdZTqgLsaf%2FAzGH9jZ6FY8yvhIN7F%2Bo4XJ%2BjPj0ys5j0yPXJrUaiP%2B4OEx7DRUyj0P9HtWMNJ7DBrnZYmZtTfy1C7yB9dGWy8bhgaRjPeadfexs5VZRL&X-Amz-Signature=c8499d7298c380e0a76934ec2634ba3181b827bdf69df1ce0bc6c7a4ebb0c624&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

