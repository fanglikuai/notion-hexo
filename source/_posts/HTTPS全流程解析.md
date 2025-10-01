---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEIMEGLZ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCRT%2FsQTqs%2BLmV3KR6UnifXOsLHwyg0FjhDqryhxJ3I6QIhALU3mjQPq2MfWgw9q%2FjMQCQLmlX%2BWr6x8iJF1ujX4B0FKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2dt54P70fIY4fFOwq3ANfUuctkLqBSgHDMGZ4IQOYRnolI3PsKy3rud281AV4OxEzSsoSeyG7WGsSJGDlq%2BN9chRCA0SUkzDR2POWJgFRD4lx3XQvHzklvTph5wl%2FUptSql7XE77Pira9iUb62uI82mWKociou9W40Cwhuxibo7PQfrM%2BVa%2FTUYpD9WmfKyR7mtODRtV5oIjz6C7yBLWlsap03Ws4IpLF506%2B6p%2Bn8NTNNK9PLgxk3FfJDZcJHto%2Ff02iGdvbDJnYYQzE3dEXnc1N5sPw3DziLwUaZH3wceQZdgfAyG%2BVNRv2S8ORZVAh9DrHIPtp2bBmwxkGzdbPZCnEgWP26OzmgzsWSiUFGwDtvEIb0NfCIWQa0Ee7uheUoPGG6YIIIifQM09BW7iKA6laC8fgOFXPFkoo9Jy7CdQ%2FLDe3tXqWmpHyTWOY7O18uJgl%2Ff3GpIvWzG6YugurzF0wvNW0SYNbGhAy5MCLZh%2FZJZXoQA7xaVpwQlmqb%2B59mpquihbm4Ykp%2Fr6j5VA7mAgd%2B7WlBkj6f9PUG2AQak8Nqe9EZgQ6bvqhj5dNslju2X9%2FYUca5FvSXw40pPGXZPaw%2FTmrPHpXWgaZq35dDMiUctsT50hfw5WaJCoizZkItjG1Nt0GIqzZ7DDwkvPGBjqkAT3akODE9C0XRk%2FnQ2RtwzdM78go5VDZgcJ6oq83wa93%2BWvKzWnmjR4pKPYvTvmGdJGQATPacSdNPudMzu%2FhrgIiSyFBSIrZ%2FxUuvPMuXl7mAvWTYAsZXXTGW%2BKKDDRNdk7oho2UICFLpJK3mszOjydeNm1PMRVILR6z1vltqWffg1632GrxI%2FrY6KEI8JxfS6uJPkRyrnqCMMYoGqB8zRVf43TQ&X-Amz-Signature=c65dbc29756c751fbef2c7b92007b2dfe369ce138afd609d9e4fb056b065cb7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

