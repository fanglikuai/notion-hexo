---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ3MFY27%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXHNYfnlEUCoTYve9O2fRmLyZzJFOq8JydxrtA1vvYpAiA3QLf8AC9SiMrNmg%2FRQmRad7HSZb5eNq1K4xfMvpKj%2Fyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMyOQzT61Z8Hz7XHpHKtwDqru6ksWW6%2BHB8TK0Z8TIPpu%2F9BSh%2BNteD9nYpvxQP%2BBrBrXpBSsMY1qWXl3IYzskTVc2U829tJ%2FDkmLZ7pp4DXxxNwBrjHXuKXQ%2F%2Bv9%2BGBEp4WSeYOBZhgR63y2Yt8NPBtEf0waDPBcyEH1PMm4Kb4%2BTk%2FU%2FjvNRNmn6LZy5M2LGlYDGuvHojq3VcIwjMbrq%2Bk5B9BRRDn%2FWuvcb0wenNlUjPoZS30qYlCXRUEkU8ZIR2nWHBJGXNjSbqhvZ9fKKHTuUc6F5ejKUnL76c6I0Rgqi%2Bv3EB8E11UfYquMhuCiZaARn9YzXwAAJdLKKZEEV2wd%2Fy5iRvTRZ0J6N5HXbTfonzkIwlLFb9Bp8ngkpomnYvegz9BsxxgNa%2Btx2YLTxH7jp4EgHXqMLbPGm1nUkx9dEicQ7PUk6G%2FRr9mW3A4wU7V5c5UZvW7AFF%2FRknwXB0qDLKXBEoErxL8NpglEFNfhRBnRReHenagVMQthjY7Wvo7589Vt4y8gaPMu%2FlkFJx2d0doRu5ynSiApjXnmiOit4roSNdeD%2Ft%2FoRheuwTkTGEK32di4ri0zXeN%2BTCt9kDMmjKvl%2F%2F2kn2K7Vrx7LO1UBXsdkNtFAPRg828WQfi5SyUWx2K5ohtPM%2FFcwwLWWyQY6pgFt1JeB8KDLwEWOvcrkJvtfIfIjB4JJkzDnooS%2BF8%2BMNZWe7QpMjO696HYt3960VLfgj5RJXbwYlWPls867osAjbykO0uhf8OKonMYcMqbuCmpqJAUFM4s0pasq4lIMmyyfrGhF57lzXmpg0IZhvSZQ0sHSNKegRPNuskAnL5iHQZyB%2BOJIePitAJmL00babwfCq1GuXyjICv4zD2iZTdJNJjpT%2F%2Bqw&X-Amz-Signature=b30b542ff857b6188fcb759c086ca6272c4c39f749c64715ec5a1ef647107029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

