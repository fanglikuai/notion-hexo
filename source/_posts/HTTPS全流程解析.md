---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632V5CTJC%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDc63EWCU005y9Dtw6iQNozxUc6B5pi0wCIDRoJYdoHwQIhANP%2F%2BuiSH2OA9f%2BvujEvP5dp%2FbcAgRIiZVnKN%2Fh4qQpYKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwByJGWdc8Kk8be7Ucq3AO3GIR2qWX3BXHRaeAo17Xrp2UPY9riN%2FC4y7g3TTANOKjgRIIjyIHLziPNFd%2FuGxSj2eH0IJglMTiIhsMr7%2FmUbL5OqHBiZ4CO8i2OYPDU0p2HYMy7%2FIWTPeEONREenChZ4lj8e3VFOY3MnmB8XyEGk%2Bdc8bZoG7D9DVtEVr0nrFpEyT3J0FbvpcA9EtFFbq7woeH8wY1ERUB%2BZvJzIJ%2BOqDBf%2FjiWettw5dO53B3XFgCBBbX92BmJntVDOXPD53zEROelVzIeIZ7ygN3m7R%2FcjrsnYxkpsMOT0KlEKNpQ2kEToewY%2BZmwcgDq9MVJgus0UpCBv9z2TxgLU5pWElllJihOmQ%2Bv5BFsR937cV0wnnHUG2VPmW9ElfimiW1EzEhM9u0zA5uJvrUvmMWn4RG3tur%2F6jjOMom59fDo%2FldaOLY3TzNnuLlfgj0%2FBZg%2F1ky8h6xfJ1DRzCYHlSuU6TC27UNG3sHlTsaDAt113Zy%2FHRRLeRpBmyZeJXE%2FeZJD%2BGlVAsfhwGJ7fYem%2BE9%2Fvx1rQrNYrx2XRNVTMg2zEZwFae75j22%2FTQq0Pp0OTLYjDNpm2oikGmbX72bMTASsa1hljMkebJoevqSo6uTFUJowYDJfV5N4rEXET3sisjDOzf7HBjqkAZXhYQ33H5%2BHKmrl%2Filnmh4O%2BFK7t1GXxODNs3QVJWaKfOTK6hZDrbw9PMDIFIjOPnSaUSGRq1b81SBwQL34qGfXReSC3CueDaBxJVR0bhZa54qlSrTULC9bI%2FUs7bvg8roMeQGP7V6zfXdfp19KqjOb4Wz%2BWK9XXruCsfwOF99f0la3yjHbm8DRqRKW8%2Fqw8IJY%2BmvBDL4%2F2H%2FWG0jgFQRzuARW&X-Amz-Signature=5e142627642198d375583ac58c4d2deb659021829093e4fda181677d9e12faad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

