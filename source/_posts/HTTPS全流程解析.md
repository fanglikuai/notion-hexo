---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMJ73YHT%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDA4Lb53K1hwd6Eo6Z5YbKBtH5n%2BZlVA3XEttHRsNCbnQIgX0GcWCI3%2BfvhYJ49mwzvc00RbHF317pIP6fkRafwRx4q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDCMI%2BaCR74O0mCUL3SrcA6M2F7o259cGLLc6884U%2FjPfifTOc8aV1sBd9tkNGZJS40IJWH3NGFxc5P8caWb8IeQitTE2oI1t1N1GvPhtvif%2BhYc06KNoFsCuJIm5SVg43Dc0xJLoe3HcD0t6a8RG29vHnkLG286KQfjizRfo8hiiAJEgWSDPEuhWVdL21nbUP49e%2BQXvhHDJhmPt0E3JY4hSPIzALmgJ87m228zFTE9IyTApWbYkb69xp18KRt8e3dWu9q%2B8So5DIjW9%2Fy47O6GDY2yFf%2BpybRw5hibAIyevTPO0lkfkskiHxf1V1X%2BxleipGPCG3y0hPJTgOq%2F7Xgr8LUEeb9FJArwRemv3abT98IhoSusllvjO51MKun6KGk21zSXkRKVo6xvvAevQx%2FCiKG%2BYyUtgho2oXnUnB3cWlCMckgI0AU1ggz7xRAfBhZigrCGhdLjy2WD5qUJS2Yr9Mxg%2FyrukTRtMisnsOesN4cGuJ1rVZsns8M9A0OKftXIfP3q3RxMCYgNIqTPiTu4yky1ryxJhNF0S46etmF36A6jAZu6PDDrmEnioasD4jbTS%2F%2BoNCzH0vi6pTLWL66rKy%2BhZn8vOEa2JjDQrMIo2DB%2Fdt05APpfb2JvLeQr7FbcEuX8s8es8Ifu2MLecg8cGOqUB7Vr%2FLE%2BDAkO42QqEmutV9duwF4hvBnAi0ZXUpbjf6LQmJg6XqE4Xitq5mRT4DsQO%2FMWCioW4jI8x%2F0DrDJe1D4dEXri%2BOg2PtIcNZp7b1%2F5DuJKZWpIp5zQb219tzJqFvqGLOFFwj7GqT0e44O0mxBpcIgThVaUFlPpnM9arH5aut2xz3soM0yyvEnaWJjMXATBNhRmm1ieHD%2Fa32eBYgBBHRgNs&X-Amz-Signature=f20b0607070ea0da36c97d0acb92f69193ee25069f0f214c298b1496b87082c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

