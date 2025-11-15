---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677UMPNKZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhRCVosCAeIOe82WBGHltH%2BZCLDA5djryLN%2FgLtfX7NAIgQyLbTpnImb6gwfIuvrARBMqk2HqCJ6YQ9CSP2YpaI9IqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAmtKFy716R7mjBCjircAwpOs6AIRuHnzMkATLNUHkRPZl%2BIeBszhIVQWc72qdBsJtC4LQW5uVpIGXBk690VJug1Nw51jXOnYFhhIL3Rj60fDDRaEPYjFXSpYqM%2Bmijz2jGa0vvpE%2F3mzKoaeumbmW%2BxlGeseYty0VAQ%2B9ItZQNSzGmOsMPgTHSQn9SoXUwEna%2FHE4V5DpqQ0KE5n4UHwMUlE9t25qRq5A2gO2%2B8GyxKPicKW0iV%2F0RzuAmSTbAV1SQXwmip0k8Bg93jEDO9UMOBb7A2w7HphwrMPm8aO%2FuROAviCrQorE%2BQ6YGQhgCFj7evXH4En4mHRJYma%2FmIUJ9vs3IVrhibG6j0YqDoZB%2Bys8F8PR6LcdGBJza%2BoyophaIyRYOAC0GUyBp2Q8OFln%2BR6hM8YbU2%2FL0sfgGUfk3pn1%2FwnWDJzh2l8Va7Kis5M7rrBApHjMUgLl7Yw%2F8%2FN1eQGZFwkFLOVxtzqc2nFE2Ddb6wJAuXzEt7FwWLET6hctE4UsDmFk03WgnZGFkD58vzz5xVLcHltXyDIv%2BlsuwwbzzMvzglRjmWCbEgcJ3b3UllXQIlrbch%2BJ91WfEyLy5tm2skqAdIjLHJzKF71%2BCtZDUExvRkKSF7ICAkYEG%2BwWn2YZqpQyJgR54EMMii4sgGOqUBxn%2FRSdE9gCDTJbK0Fuogd%2BDvp6LMhznh79i9TwvlOVBZtvcXt1sYDsJTzqLB1a0gyTXXDCrvuFa8gqnKNl7hHarWbWP1uZj3Oe1AiqETS1ZAcpFgJmN7JkMtfbczyE%2B%2BqvWqon1HOoVw6aRMErqKiQ5dA6puOBIUPXdhLpJvNimeIxrciL6hnyV4pjLfINhDBSanZZa4RN%2BuB9v6aj7bCvYhuMEu&X-Amz-Signature=be41e94482e739db46fbea078de0c0ee7fa3a6ef9334a5645fad7a53b4b7e415&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

