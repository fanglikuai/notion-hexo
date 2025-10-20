---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FVWJGWR%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQDDcOQBoFiRN5v1pMaNDLyyHuniFBO4CmEkwVSYBeApZAIgITNcKedurbhpIXddOLcJtYTscQNKrHzMt0A6WveeixAqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG8DfX%2FJJuE%2F0f7ehircAz8DvwIxCFI53qlFJHbcU70m0BbcxwzR4WYS%2FCiu%2FA2j1neaEoJzKBJ9de%2Fi%2FbaxSOd4SIf51bIjYFlK1Bn9AOHAfV%2B7bekWu6rzzsDdcXdpGJ2vbEYOLTkcbYEAR4lS4zfxq6zsazM6oFa5j1eCNyDeCV6%2B0GT7A1%2B0MkO%2FoYbFTV8cRr9I%2F87UxlTJ0f%2B6uPP61Of1SvFrQGXtgvY6Qg%2FuW5r93OV0aBvOXizqCWPhDccfG%2BwFJ%2F%2FeQou6mCUcWK7TwKLINgDvyeEVn%2BFM2R9ZzOHPEQunI36tp5cV2Wd9Vl%2F1nLluxmj2HuviIuF%2BekJqCDEcCJY80lpNy%2BxVCus9XfNhF2NM0iLAWz%2BaTXeZTjY%2FTvThNXnl%2FRYTCzOoXrYv8N2Kr%2Bn9pm1idAiSJtYh4z%2FG%2BICKBZ7oyFDGHl0FRHpxF8XsvaQf2tO7AAC%2BwxjvnKxLrgAbr1Q9%2Fs2ED6Nqy6JQATKk4lQTLvB6XATGNRbpcSZEQa%2FOpk1EKK6l4AYP4bKrBsK2zfj9pHbxdfg7yq9Ks1I0kg4UhpHXs78uMuTKENcrcoAyR3ag1e7Gz%2BEywuznWsUOXqVMjUkfCZ3vIXFoSus0DMAa5O3HnuE6XX4FuewgBR9y0wh0MPm218cGOqUBrKV84JMlpqW1Nwu3gY3m5XMarOPDAUO9ril1d7aGiZwyVSzDEJ3ZPC%2BrfP4SsJPOw2%2FgyLZfeh3yE18Xl2OuQYfaogipW7lPtsO4qWqRPeAoiisyrQsJkm8nrnwmVbIFKbtbxGQzdWjmBxjkN3%2BiIvxHwGqcDDOW2nMSaNZ4VS5Xvh8VYDg%2FzURuJr9HqhvDTfpZZzpyBfYN7TPbJEQMj3uDApla&X-Amz-Signature=fb0b55a2c44ecec342625d3418bf77117a0d0e36ed8ffef85ed9addfae417c0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

