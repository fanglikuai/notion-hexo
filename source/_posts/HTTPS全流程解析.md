---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJLSJWCY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDc%2Fflo00taVOfLDleI8gwWqz%2Btf2%2Fcteg4GWLvaAUMuAIhAK8OB61aWzTvVxUEFsiMTnv3uPtg14eoY4L%2BCwD7b50PKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9NMgXchCsN1vsnA8q3AOWfaGAYhStN79GruO3DIYUQO6dRlzN3wCsSygwlJX7morTY%2BjWqoK%2FNEgm%2Fl4Tad95iPl9DgNF1ycYQfN02ye1ygjpp7SJWQF46f6ti1GVy8g64eqOH75Gwm%2BKujL9CLP9eOWO%2FZ2xBLNbNHhKM4CIP9fU%2FTRYIcCAH0jTNKaDtDnzuUZOGcSYaNnguWj5oxQvip%2FHgyJJKMwdoyHPk4Hwk46w%2FUQ8E0Da%2BICt%2B33C8x5pafmyxncJ36fLrAAuan0ILiiEJGUJmtFPffS2vZHJT3dTLVGMwrB2TjIeY2y%2FIdIKBgI5TBqvtUSwTqESUGBt%2FpKm8c7KBOv%2BFVFPQoQdA5mj3nMQ53%2BUitPbDWFKMB2%2B4s2U3zApjFNewqmOebSpqfGLEkwDSAps6Wt2bWoBeMZQ71%2B0d0tDdkgT3x0tA0p1mMipsbMg05%2B2QgzAU1P4vVHWuor8UOrdeN4SuVT9JnzK5l5NlQGxV9qrOX2bQB371iHQDTR8cYMVfHr0%2BIWBagG9pjBJJqPyP7RND9ZBJG56OY%2Ft%2B%2BwdQq9UL84FXuiIYAEGJPSoy2ltV%2FgN0FTFRC42Zvr%2Bxav8Nfjs9mWThkJNukJcp4SnaeONoJTGTJ%2BeugYvsidcGqlgRTD0vqfGBjqkAa93djRxCN9004MpC9Kcp8kZi6Ogjm8BEAgAIS4dK1bKu%2FALgXr4xugE2whKUypxBuEnkD3juVUeIVQQM1wHYYWvY3p593ekgkws7kLuxApLNKawMTQDUm0lpC2mcK8C8DA0Ls15r7ATVhgKhMeiMtEPO8ebZu%2BKZ2k7AmSEcXNXIsqE6kKuhpno7RV6cjH681yVCASvHMd9Az6DDo%2BZSXvRbrEi&X-Amz-Signature=8fb612086b3464f053ef121799d62f3da037e84ae1ab4f1a75aa7ac40d527581&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

