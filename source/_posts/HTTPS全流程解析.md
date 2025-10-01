---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3R6K5FT%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCICK%2FeuFlPXS5r2wAaUb1Vq%2BDTGM0Jt%2B1%2FMKHlA2xe2h%2FAiEAmE%2F666PTEA7JFyrq8W94b2MNMtdGkxbt5mfw4227Io0qiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDImjtVKN0qgI%2FSuDkyrcA4XvqSumICawkTXwf%2F%2BcWCVGdYwdhbGo2DJTb%2F%2BJGjH0V%2BoXe8r4G7IccnQWOCeiNRsLL7wksW8wxXMA1LY9q6kcqyqvyxndF14Frd4iHneUk9o6n1lF9nNMV29s%2BhOHzjYS9b1ZXvl6SNuKZ73nS1bV%2FbpRM%2BVkHlOsznngHwm8KTezw5n5LgLMIimKuA8nHifDxTh1QSQc3QPfzpTEHilljuz5ac947H%2F22zwnImID%2BYnOeyj4TQI8uHQUgBOYQwlRcsQizPwkf%2Fm78aTJzTki2c0QYkOBc7ucY42JVMI%2BxIy0yQRAVaHU6ez8sld00gFXwSFKnc07Ew5Ms4dLQIFqA1ppCRfTxw4XuCa8FgEWnZHSm%2F8AtDNki3a%2F0fQ4o%2B9Bc4trzzocglstIgj6Kr555dZuToTnJMzCKf3GBvnRxDR%2F0qiK6khFtZFn7RyK85d79UydOe4U5UwB1LU9GImvCLjyqNMgsm5mtUu20%2FpzW2eRY9j9%2F93yYax5F0kvjkmp0D8lKAlC9ySWS8bEqRD8YSAGAqRM9Co0ZR4ehgywbOUX%2FUmse%2F0n%2BIoUJRJUSbg7e9sszbq7KpNsstjYnkLZRlC%2BHzFUWr7ayNgXCH%2BummAfzx2sMKP%2F5UbDMMHq8cYGOqUBrVvxRCz9DVxAa0T7f4eO5J7nYe%2BEHt1icM9snI0Sa4xsQoz9Ichx%2FkhQ55wdlAkKegkNnKuMefkU1uwZj89ECNUB%2F7dc%2BygP%2BWbNqm%2FE%2B3rlUJaTB4MMZ3lSz2IUhwR2YNoKebFkZpoGeMRVFiPJrCxBNBVSB7AR%2FT0U2iM08b%2BjC8Ng71f2psUbRTnDFeRiyBkzdKinNatCQoOm0B8K3h8%2FTxxK&X-Amz-Signature=86806846ece248465a20de334fc036cd0a3b45cb3d19c03b3da0c2f1f79468e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

