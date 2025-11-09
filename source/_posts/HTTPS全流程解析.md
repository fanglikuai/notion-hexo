---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7RGK5U3%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIGa4xc%2FXTc%2Fics3fWgmMzUK6BrKYKldqSy1%2BoQQiy%2BdHAiEA8uFYwkT8O3Dm0m1x5loyMibQCRLeYndnj16AYLCkoTcqiAQI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCSHKHTa0CpzPV9e0ircA8UwAk47KwXx21mWWE4xTP5kVZ%2BdMGnAZnO1%2F2GF1eACa%2FJTEPxLxBI5ovF3XH1PER3ky5RJ68ZHVhtjNki%2F7wUgBa%2BAE%2BFjiIvcDYNxxsheETxTx3D8%2FM1E%2BJ9iyvARpM6yznF2ZPeA8lbmv%2FJzmqeWsVxq1lx202%2Be2FpCobjfnn%2BYR8YDb7III7XmGqU9SZrbESz3JQzzJC9mViXhKjpYywiugSOPlsUWJFS73Ge0OQhWdHd5%2BnyGmJVUBl9ed9ubryZlGmAnrHtp9sBKDDx%2BiE33gZwkkAHpHBGLZADvfAX%2FVJpE8FRj1rmpzd2gLQVQavtYTAZ0Avu4krvNmSYGndXYmkKVaoMHTeKycYXsAhKZ5MR%2BaGPS8nI6FnzKciFaiGukqJP0abYGLaflO%2BQW4olW9hqTA2ZZiobtP5B2gafWNrHtLvHDHieQFJp5V1UNgFFu0uxE%2F6XwDJEb0EpqBjAyAZVSD3mm8fBnoQZ0UXXHKd0ltptJethXk5P7%2FbPl3CsUpCibbcPQcUXuHWm87Cfu6u0e6W%2BDUjnxhbgEB%2BAK2dduHQCIseLNwTm1G%2FAMoF1ytsBYmGM6Ap5H0XyieAQE3AK3k0gs9z0noBZjsMYy%2BgFgTuakDLHfMLCxwsgGOqUBsD0X6%2FTagK6C6RKWsmmowYuSQN552MwaJ9o2U0U9S0aglWBnYbTA5juKpGMRrSdY1oBZaMBfM%2B0JAvIKg4%2FNj6%2FO44Gx310Dj5BTrYumK%2FQ40idowoIswQM%2B4ZtciJX5DxsnGsLnjfu4%2FoDu79AOICV8bHTZivf3oL0QlFuAw5Alxe5lvJa%2BY4DQL09RwgM5OjTDvjitPPh%2F0%2BE0XBQ%2Bvb7ObaRJ&X-Amz-Signature=47faa6fc5c1748ed7b89c478a60be28f7ac7f3de427058c21eab6b50be9548db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

