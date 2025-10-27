---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCXT32D%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcyWEh9MWVLOnRPDrM9UU%2BhY%2BAY%2BiM08t9e05KTEL1GQIgGM%2F%2BHut8OjDZf4R1dtBsxzS0a3CO32R26%2BIw6znfY6QqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOeapAQs0y0vZtOcoSrcA3id8eIrH4Q3y9tI9t8LffEdyKin7lJfUMTRmaLKxQridvWdh9WDQo2VqYyIDICsdQU1hM%2F8MIWJ%2BB%2FiTf81gPpncTo%2BJQ431ieQAAQ7VfbY4IjkJ6%2BQG8CLuKejGufP6rEe34Bwd76qMVnaNXoW4IbW9heKrk6tiWI0Vu2QE%2FQGxJxFG8VyWSjwz17Xu9kbD%2FiI%2FENmfaWsy5g9H9lX%2Fq%2FdXsv0di2CE4yzhwoPoxQNZ0S38ZvGr1vD%2Fet0uS8nymwMzZdMj28V34ZhjALtud%2B1DHneNK%2F%2BspCF0yHI5uMZulwRrtJpiTVzLgsSqUEVQDEpBGVIsBZfFZ1BcvuJAtoW2AnogKIDrR8hsHZjkSri9YOMlAhSDB4%2FYDJ%2Bqkl0o7Gksg6cEObTxbzOxJgm4H4Dn3xoHcd8edgr0sbnFyxkUfUcZXSdG3MT7F50E7XFPoJtyW193iiUfjyzMlm8Z80e%2FL8SgIE2PqTKcqXtiC%2FQOiqw%2FBuTY5vXI19pfEFqfBUxeCtI8KTaC7c0Jnslktmx3Sb2058h3BpEM1FNAaHq1mQfwJSgtc%2BvuP7dS6%2BB3crO%2FeRupL0b256txQWHvi7E78N8jhz3sZmasNsA2LYd5JhCLGULLM%2BnIRQNMPuR%2FMcGOqUBbiQAhcy2UEPF6woHoLe6Ro5xF3bh%2FXs%2BS9YSZpXche9T2ET2rjzav5rG6BlN0nLcMfIgOfiQoMYzUXu85luofYKGuRTWXpX%2FuG1JbCSAAN2fSDNTFl6vpIg%2BGLkb%2Ff5Mq40lz73cQUS8c6ahmH4s5qm3PXnmw7bjDX8oywv9giiyzu9OSDI5VUb9jClz2Ubm%2BFc6qemmOY3yWxx%2F9KDA1cg%2BTjuq&X-Amz-Signature=ac2c345ac80b82ab111ff3c69281a994fbd84ec76ee344fd842c4aa6c80e7e7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

