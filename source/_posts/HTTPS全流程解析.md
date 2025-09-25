---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QOPTCRL%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T180114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCxDspVErKYxQXzt8cEw6AQc%2BkUbWjY2XDWlav2%2BO7n%2BgIhAN%2FxklsPet5nyyLYlFziNJys6XDkuQXble4rT0XGnF3TKv8DCHsQABoMNjM3NDIzMTgzODA1IgzJVBGL19cp0Q3hu58q3AOo8UvZCPZFniXwMTHzU7Vio3pUHFQgzmG%2BLIG1FhQq2XWxj6jhQGawSksVw07feS%2FiQYnlkpaTctAPejj6zB%2FKDsdbQoTSdGHLcFpgVdFOgaZPZ5xlrMD5551oiL7yNPvSfdOYAjG%2BHF2AHJAZSGW8B5x9vG2Z6IN%2BNZNtqMCL9f8Y3vDDVr0G6RMttZEz%2FfN0dMS%2BxblTBHPf%2B0HtOmciMlztkxObmtgq2y%2BDI9Ky4y%2FdUcwS83U5SrSlI5WjkcUQ3SS2Roj72DHOurVb%2BjGBw1YgpRDNFCUnqFSvfx75ibN98yZaO7Hn15VWSgDRaZTrQhwg8zyDvm4dO%2BrRS0hUM7MI%2BN7N6GKo%2Fv%2Bajdz2mU6G5YDRAY64EzSuZyAYg2mDqhJK41WdTBzOtYhx03ZoYkkSk50plAun6HeyhC6W2EUAgkllDlDR4twjFguD2w%2FbSIj6Wn2MbMIjKgh7FTY2f%2FFGsPM0aFQKmXV0yyZDj3tNMs0RQ9%2FFkWeR%2Fhpf9Yfl3COxYtPkAzN%2FPuNEMkMBhj7wiq2oElHc3KMMlSzRSyjB7hni%2FM1b%2F2qXqCE55%2B8TZh3dDBPz%2BKjmQ9TOuq%2BRXfzxZ%2B7A8wYJug4Fn1kNuRl61ixYh1FTZJoFtzDi99XGBjqkAdDvZ2PPUi0E8IYpKyIt%2F8Eo1U9T4xx4lX8sNkmJOai4chgGqOn5rZthT2j4rr3g6kQDz5%2BSXjWJloSPPJ3yxNRAkLmbgVxsOssVXy767XVNE6rGu4kCAVlnKPr9NjEPxUVUwxMgGHoyEG4EYSwGB2G%2FqemKXdUun4g%2BIdQZzmxbSfySGFBgpxttXhLT7VN2IWE9FDvpz1teTnFePlq71t%2BKi%2Fub&X-Amz-Signature=9000999966c779040c8f590fd024ecc64dd252cd588bd814df6360f5b4ab402b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

