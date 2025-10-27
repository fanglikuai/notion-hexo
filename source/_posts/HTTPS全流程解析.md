---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGZTP2Z7%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T220136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAbHKxsodESw4xdyahhnTlpSuB%2B1Qlw9zMnmfBQrWnJ6AiBSzjlcXLoAvHDJJBjyrpPwiC%2F86CO27ZkJl%2BOs8%2F2bpyqIBAiu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuRk9IIqIX0OzfNNSKtwDXD7KbGpPbcSCdMvL7MgEzt%2FSkvQ%2B5DmnhQBqOJIMA4gSgUTaDGmn7IkXrFiNEiPxGBCxiktZsSgikHiTcZRNlKK8P7SJ58nbiuVgPG6xn92w5tcvlK33r729KE3E7gh9AyccRPp31ObaLuuldCyBvn7oT18QiIvUu3upTKFILC0ZGdfHG7Lvr3m4iKTjmiiBwF0NsvxL1RSgoXfGJ5m6NVu1Lrq0E%2FZouiat8MMoJ8oEn%2FIuOPWh94oUsj18YGUo7yUke0BxnWLZOnHOlWSUYXU%2FQmqs00xpPIi%2BjE%2FoCXVo%2FPy%2Fr4gqhyDZCtpJpEiZBB1WngM%2B3c6Qsw2XPDWk%2Fu4ge5SDDRI1WCUt%2F04icc3raKyyTrAx%2BVUuz16DnA%2BI4GtHkzNpqSG8%2FKWMf%2FwGEX9e%2FBsZ16bgbfYnqRmRJUR25Clcjk23nv%2F8QjbGpnnbOcj9gxAG0oIhJ4XhndXZCLJt8JX%2FcpZ5D5RV8dUw2GZMjCDvyN5iic24Oc81NqQPjVAchYN9kQCFB4ZxW2KjeX0OFZHyojRcK6lWzPMewsGGcXMttE0xGHDTZ4gRtS4kck8iRcin0U1sNjiC0A7PFHQB%2FL0zLdwAzlHjIUTPmi%2BjJr0gGROv0CwrepMwjLv%2FxwY6pgHTsdNQ17im8Ofu0mZDbxc%2FPPUdTXitmMKfjucEJ7XL49ojn%2FfgzYrgUbQBsetovII4PLoAavzQMpw3mloFPkn1FfkDJ1E3jcP7fKcNwsZm65tF6RlnhGGiQJV4a4yzoGEeicjqTlqnxOdQIwdfLRIX8Wg0Azyw1ePMXLj59ADWrDXmRjOTH2GLnbRYmpXhbBkqVML%2B5e66rK8KZAfAM02kRwyZ3W8K&X-Amz-Signature=46718d6c4d371b3d0b891c3c4e9957870a92ebc1996f6a4953196edf986773bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

