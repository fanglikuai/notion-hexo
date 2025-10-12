---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZFMQC7%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIE%2BxShgdyLC5mWWqB7Dvi0Li2GP%2BskNjWNOFKaBcrEOZAiAJqcv%2BBmrxkRojLzpruqXaln0ilJt8kL8t7%2FwMhmZm5yr%2FAwgfEAAaDDYzNzQyMzE4MzgwNSIMt7Y7vpBBxWN2yAvHKtwDfYnroJvP4HSTtgEH9w7ifcndWfw4ROSgFFDWN%2B%2FK1VMWlOnM9SGHEzhwIjWDWn172lvUmw8Kj72H9WY1W5a6xqrFh1qOwRf9ks%2B982JHE5d28f4%2BfcRu8%2BC2ZLymG8fCIJsqYvmlxmzsHvWotoK7FrXgl6Y33%2FbH56rkn6Cf%2B2pbKTQfXd8rszSLB9dy%2BAZt0wSI6oEqDKF5dapa6rOMBJqCiq%2BuWvYXB20TIoNMQYEX0cnNJ8tEbD17os6QVoDYHcDE%2BQyCBss3eDbzXqtbkA1Y1SxyZZu8NI4GN8dfDCHNxo99yyhoE0LGut1jv6%2F0FAFt1OVpLYu%2FGSw8%2FzeBq36SzkIQHm130%2FcvoqeRy4ZKdFqEazs6pFM6X1FHdXZxyG0OOOIp9AazE0FN0kE%2Bzzq%2FRv60QmUooZNF9topqP4p2Ashc%2FNCeHg4k%2FvomKXutyBT4gxsXhoX%2BBw7XgFAZSQfFXCgFB6CaVB7yGw7PTewDLIvyFDBgxxK0wYmQVbL4JqJ2EVxx01gUEOoDTw0gmUdwI2Y0lnQ5jy7g2kyOPUKE5eOz%2Fk6w8lmfxbSLCqCF2VLkcezzo5c5MB7j7xgrtX1vffFpE%2Fv%2FmzvTdPeDYR8m2HwiMZXtEJYK8kw1KarxwY6pgFSTMWUt5VP3oGX2%2Bu0R%2FWogtpxbmRANK%2FNdtYbP7nsZ1yRAi0E8t7ZUYTg20CAMelXxwN%2BHX3P%2FPNzw5%2BM2OXv2opl00JKbvM7hpcNZBFqi0UBSpHb%2Bns7pDAVFGo%2Bd%2FWBRMAwDLrNxZMAU6rGgUcI80h2DAjjk2R9YQxFyVdvyW1deBbrNd8Vmo73tIasN%2FdagMTzVh6X0OnpEWNu8fstJUHdpJcA&X-Amz-Signature=8554f0f8384e4937d45e71bea9ab3ee24c57b9081c7d2b3aa045f37720de5063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

