---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EFWF4PY%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIGzSS58ujsxeORYUTswkEctP8ulAI18k%2BbGoeXwjZzvqAiEA87OEnDCGmrljauGRNUe9a%2F7ihBvUh%2B9ILBZSXvypHTgqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIxbel9EYyBmPCQhCyrcA1lDtPlRtcg5MDfd7zHuczUNGMifcG3Wt27TVOMg0tseNmaPKLmfAs74%2BepB%2F4EJb0R66es1hdCxm%2B6Lg4bAr3YKxs%2B9zPrlMY3v2m8fjUB2TpraTWq%2Fx5PGqlDOYSkRr94Tf00pBw4yadT%2BfAvutp4XmSGYPJ%2Bwd0OeQEZyTAtZf7Hf42e0BoNeKPSdXalrT3uJqcdH%2FM13ztGTbdxibKPwc8S6fMdTHEhq2o4FN8yJIUxLWMMlq3tdRxi7CP0y%2F0ggPphcjhqMQBotPglE31CeaiAE8YSbu7AAPqeXdhbypyVhQDojuaCMg9nyd%2BKm4lbdDrwjOTJSePMowRkT41Ke%2Fx3yaXQMgr%2FWKSf7zeHzkiPJfIWYcQHX08anEBaqMnRb5pGkqyFrEJtD9K9k1X1aU9WyeEtjYfoprnRWy1Z7uOzYdk6KUA4Ev7NodvBVWd4wAT1%2B9JgUw20snLOk8LEp8fz8tJ7OOnq3BZqwtHz2mxuB%2B7ygUcvD9crWPl0eu1rROJmPGusbkZ0N9dj5HkeS%2BR5hW8%2Bnahr%2FL2BfOKd0yAPakvRYwB5I25Kh3aIhAeFXyIVoIKCYmr%2BX0synEATNfBw3cNkISwmSpYXYbYf0B0ZATSTDoxp01Q%2BDMJ%2FOt8YGOqUB36Jy27qu0xJt5YALq%2BmJZLIhMqP%2FdhuZF362DaCeC%2Ff5X3EXj8%2B8yw54OPsg4S%2BeAuTCbsOb355N1QY0dWLX5DTZh8zMGIfTri7xGrwbws%2B%2BaF%2FNVHWxkvfjFFkSL1RRgxlHiy%2B0sbRwTEgbLbRgXOOGlppe7LwtAcsOSQ%2BCwXzGZO19Gp493PWiKQf1TeMOfLb%2FpxH1AaK8nzPdem2UZfJqtrs3&X-Amz-Signature=9c9dea17251236ef30dad4af630dda1d038e5364b8f81ffc5fa347a034ffaa26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

