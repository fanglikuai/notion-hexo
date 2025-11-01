---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY77H7F6%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIGYBk1Ah3rTHBJuclVYUxXdXbMVeFZaHr4ezWXA7%2BHeJAiAqU3VXRV4FlNjjlE010QG7%2F2uU9bN9ipn8DUi7D2DZgSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMHfQ2EF45bpwcWW%2FpKtwDgcL0x7SCcoqRTEuowV3blQzhGncP%2B3s8lrv2umZk8PgyJYfrklNakkl5W06dZTumP6cf1aWnz7gGPdvjpIvw59U%2B%2B4hCc3r%2BS%2FU3kH5%2FZplAXmNj1O1wHd2nQfffkvxaXsA1fXPdSx0t8ObCpoX2kv%2FDXQIJb8aFmIbCsC6OIFoOsYwHNV5UIQ2gTfSw961LlwKvx%2FT11wKAAzkl2xh4apmClEeHq6%2FNRNHR6Q8DyCzxPIYZ4aacIjpyzRbL%2Ba2dF%2B6oAXJRefgrBob2Bh0WpnoXfH07Lbnj7rBCmHd9SRv7ECKHm1MhNWn4Sfy1czptAF%2BOreyNMZ19zQL7ZZ74TyIxcUSTev12M80fvy04pg8B4IUwrSBLj6yJs2DfXVbCF5MH6Kvu%2FZi12tf9aIqEzxJDv%2FNLmF3G0rUDpWe3daqTjs%2BXmgRmPj9vUDtD%2FZ3qmrw35s3k2HaTPfL0LfBOExaWO9%2B9fG2%2BhRbPxW8FOzYkCxoVMcysAbCk%2FrXjbMioQB3f7ytMhWYR8fj4M5YSuio7gTg8CyrM6BMem7ByDAmKlMgeVrdvYJfihOut7Fbbzx5umdPWMy1GFjffyIML7jGpDBPaU4YWnveTFMkCB9h0wqNYbL6xSS3B6q4wtOqVyAY6pgHV%2F1%2Fq4HGEeNth3M%2BoDImDpVEuc23GNqJV009yXBs8HVNQZjmlxAp2Ukb0meSA1F%2FbtwMhNo85YYJc6f9xh6%2BWuRhA%2BNsG19zKsCc1hJJc5x9fWiSexLM4vBxPtQCdqpyVIqQrk5H4fCURm3UOsxw%2B%2BrjdfddNyM%2FVbke0GYbqrYJNHSPM2CVhU81ff30WgPA3bry5yimptnGHRMG4TcKRx2cJPL9d&X-Amz-Signature=b7450e98521a30a6d42c81ca48bbd30e36651f9f49cc12a38ef6b84d171cc488&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

