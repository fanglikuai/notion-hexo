---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZCCA3ZA%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T125005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjYPByxsu0edPVV5556dqW6S2dI101rstjQPetGfiQ1AiEAnunqZvN10OiWI64tHl%2FxeDurWlZFGBVOusQIDkzYrf0q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDPXZmmC2dYLIpIjjbyrcA3jqw7epkyLYE8AT%2B25YkeAEfY5Qp2mWPQVRJg%2FfFs%2BDlHy2mKG%2BjUauPHgH8SpAEoVr4S3NUHSxYlrvQfbBMQ4VxF7PdZcQDAuvbUlgJ9yXlX16%2Bcc%2FQ77AcsTHjlOIXzYqTAApk8WOO8jo1SYWf68%2F7uQnXFFFfYJZiIXhFEmCht9cmTZ145Nh%2F1S0nwkPfZtNHt7Ob85jYl4twf41UHsDcSPZKdYPfmoDNLeNyyfHW5M8nK2gS4td1iedeeJ3qBOwttJkEBBp0L97SsQbd3z7e2Q9yvRaIVHixoJZbSj8s3HKHwmJ2X6Ko5PC%2F7eteJ2cstmynrp1%2FUiJjoLlTOWOUrFJGxhrh4R7%2B%2FYGRbmkGrJ9M7WIUu6x5fB7G487hIBjUuFkcOlJxfG4HXBUmprfbPPSNGOu%2BSexwWmHzu6DEY%2FCFFcus%2BJ3VhuH1xl8aOWUxyg%2FVg5Bx4gU6VvXYk4mxVWqEoIuVt18pMelUqRhlNmfVA1J%2BEuVaRVlMXhy8m3BASgQrM%2FuXangLJNbadg9BJt9k34rwi9HSiJNiTBQhYyFq46Tqj1ET7fd2bdGNvfUXO1UiXpomza4ZW2Coue22a0HU%2FpBMTijYAJ%2Bf9apW8P6XwpA1clbYb5pMJXQmcYGOqUBqdVfALQD51Ur5SQMOnHFmYk5DmFQA6CNjxH%2BQFzsE%2FXl31S4MIpWpvOiP7YzmrK5dc6LotChteTqQ5ReAy%2FTb7fUyLZa7i0x%2BWIkvpyUN7SeBBJ7MBgSiThp2eExKzxynGTlelZn91s4gc7Z6yzwHoOLePaehQSeC1jd3%2F0kavV6xKScAW7uKaGOldTXizTbtMf%2F8zQsWs8l%2FAXrTaOQ5E4uiszj&X-Amz-Signature=4ebbdf2403a6deaf0b4ac929ed57ff9f0a545510b6b978cfb0fe72bece4938b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

