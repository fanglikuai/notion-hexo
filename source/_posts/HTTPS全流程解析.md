---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDPAESQT%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T031821Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAvhpKcBF7n2%2FEyOXeYo%2BuVUWSWHOXsN0Fp1%2BdKfeiknAiB0%2Faq9wQ9hzBkxB4rmZzoeBe%2FLVrB3NvVM5lEuU4jCyiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMK3BWlkWP%2BBDcl8ysKtwDD3v8EPofFg4OcgodaIE7sU1mHqukQ%2FXeTpgtyxnVLyUce31DJAh9njGmPafcNbjOyygvbGcJzp4Vvwk0hJfop%2BNco9RVjppOPvFqX6MPd%2FdnMCzUFikp5wzSI55TYPzy9nGq%2Bht7KPlXhOormu8g1vNwUO38BVzBG4r8qJEu3xgMK3EOfxVahjRmwOGwxOBInU%2FhU%2BeScyJb0%2Bx%2BL4ernDnfviXMNWC3iqCDzoWNr7JknO1btKzBIHRRucAxDOnZR%2BL%2FocimVkVsfmVzFlnjdK9P05Nc6iLNc%2F%2F90jXjvKsSqNImiLXiWGig%2FViD6ZR2FCL9md1ypny6eGRbwXf58OQbAoNbJfPioFPJbmObpEItYef%2BvkG5xfP3ki7xq9E%2B8sURyEi7YgsSzhsgiljyZmTn5OwkHsYD54yFvD%2FyXyqqAPXHhySkr6Pk31JGQ42LRCjJLH5KOW6fzag5N3Bp4UQYmAjlX2ZlauARfh7BJdI11xtCegr1v3E9WMiKKOsEZGIPXpjZsM0QBywA%2Fj%2B%2BldMhFntEsZzqvhpvBIiIDQwWJKU3T7I%2B5QY4WaUSivuUoz5gJMCQ53lbpABn8y8CXEMF9VoDIYERYiKT6U8NTph8CRdI0DW%2FUusbvucw0oKIzgY6pgFhCo7Q%2FPDiA8pkAuaGVG8tglLstIxmaN2sR9d8VQT9Ch58e8rKLrO2lZUkWFyGRfxOiBAuiUnTXKxI1sedCJT9h41fzO0E0TfMkgNApS%2FS5zYjV5efJmbgozrAu1ZwGV3n1LtW3fh6wMtbrHT%2BJ3QQoAeuahiMEkD6usPGv56qHMhiZfp9ZUzhUohm%2FTEzE3uMq5aGyKV5ajtj2UqX%2FV%2B1RovwPd%2Bk&X-Amz-Signature=4638caddad2b5cc53b041cb014b3d5056fceb8dea1d8ea18b99c0a57618a60cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

