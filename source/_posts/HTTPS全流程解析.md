---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YGYGFNKE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGnkwloeSrw3EmIlZw6p%2FiGa4gMItg3Z3HudGw1FT0ihAiA7GTye11CdZ78pRjs3x0YaRCVG8PNEn7yO%2BnAJ5q81KSqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtWp92HYlnOiVBVbtKtwD0jpvCx6gJ6e3sk2qAssX8JvRMnnDF6%2BIStULBHzOl32er41FbFyVYUkca4HGqLWDGQCu%2FSh96d3CIMAYYilSoQiZX4W3iTXjXc3IeN8kJkk59xd%2BZ7GVpt0N%2FeLBSKhmzCZNZ3VVZ5PdSysog%2BULHg3%2F6AofINpdLcv3RbH7sN0o6FmRnGg2Z7858buPRTDNAmZzsfO%2FOEN4hPeCf3XldhAMjGjcxAUdpuX48jR%2BAwJIkOY4SE4wTADQc9wDupX%2Bxw%2FsNiJsOx0aR%2B11AKim5WczUbG0LU3bYCVYXr5gF6ahmO9bPUQSqzzcYPBHV5fQi9ool171ATKxrjVL%2BJI86JophzPElkX%2BuFSRq2ETtzB7oSazzCE%2B%2FgkIO1MsrUU2lB5C%2F1P%2B%2F3H0px7fiqFugItss3NaDJWNy4JvXNoWVFRs%2FE3nZ36ZwbEW4GWI%2F%2BIt4UjS%2BEc6b1ec23cHbKOdPNZecbUY%2FgPBpEfd44OtEdiYkpf6lxmpj7e1a3Ed929WE5Ig7glyVIbSnlzNS1E08pOusr6%2BPsfatwOfGwrky%2FyIbESNH5MxTp%2BGIfUNMV%2FWq59sChN8IQs0oTFmRZ3GYAdN5mX%2FvYszBYQgrvBjZ7FhPTsOhucipbDzJ0QwhID3xwY6pgFPUZUiZoILSRqi1tUWX%2F4LMZBVFxHXx2PxdtJntyn2ibTTO8gm4zzSrhAuR6UNuuurtmg%2BwiOIARh0hpkFZZXvpG8ZMg%2Be%2FASDclO1pms%2BXOcv%2F6hHAibFh1a05Ma7WzdtfQNx7TlVY7aL9ITkBbY3Eq0vEU3IhFLZUXk2g32IjClag%2BEIzkhWV%2B5idbIsjeI9yuAcg3JIcEfFdao0OKr6zJumkj8t&X-Amz-Signature=48e317ec2425b3719710ade4cd4613237d08fc01476aba9309b171befdc6def9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

