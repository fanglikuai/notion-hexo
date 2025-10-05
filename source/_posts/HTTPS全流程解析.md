---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMR6FOCF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHIx8VstJkzaerLDckYXD99nlumRdtFkZeQlElFMeIm5AiANJKd%2FBoAyoV3y3bE0Zn3paPXoBqEhACiEKYnmLWP36Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMCq0w488CLdbQn68HKtwDSSEbbV9smNkhkDmrIMwgtudmZ3hESm9H7CIbOAQzI9NVuEqXOVBEGlnPi6MgK3JugiqGt4yrKFlP5uUn%2BITCr9lNNYKlLHURqCpHo8%2BrXU%2Bzi7FwH4qyL8%2B3A9K2ydfD3xfppdU2c7K9%2Fv45%2FlMt3bZ3M3DhLFohCRHSsm9Yvca4gn%2BQk61e2byybNADZ61AIt%2BEdY0yUsgUQUFb8XYq%2Fm%2F9GwGkOjwN4BCgRq3j6H7bd29QKrbrE9jyM%2BY9yaPyhK6qlAfvuRyStsfvdLBObvUQDYEYp1%2FS3iAQnuX4%2FZcxTje95bdCcbi%2BD0L7pdGHYsGcovWBXXRl80rkmRUhZrGS9IcRKfvEk5PzOu1ffSFUBsdN%2BKtiCD0kASfR8lCpR%2FGOqK0k4III89Sfp4csL139Fdl82O%2F7Hyh0FnfERzDw3BPCtmsgKGXr7drkucueXQgKUrYaFZrpEokrLSEPczpa4VmDQNC9pmdr%2FF2zcG67cgwzZTDa4t%2B5OKLVlM1bpPtGZj0PbpmwU8YI0tePm5AqdXpzOxKAnHwc%2FSGfIQPSvq4wywjrrMRktZ%2FrLa6Q5zCA3QDQwJG2bSKlZuI19DaDw1GKuOKubNLY7%2FwDSCPNvcO7n8FZX%2FxpPjEwqYSIxwY6pgGlplkixAVc4UUTiMXlpfusQehhWEekMOjWNzbQXuUFd0Km2aNirCTXtzWCE4zyYo4o3FHnlm4ROr1H%2BokPlwkt1%2BV2w8qJQcb8qh%2BJlQGQnpSQYjeiXlfZN6HoFPoy4ebI7%2Fyq0ssN92yr9H13irUpuUfCT%2BLNgncn%2Ba%2BSjk2U6rag5wm40t5pzLMKbiOufZQdettrmgQgsIq6WP%2FyBGefDxEE0XA4&X-Amz-Signature=93dc3853dacdf5b31b4e5eff2c6134d1ea579ca02aa6f507fcc4365bcd170fa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

