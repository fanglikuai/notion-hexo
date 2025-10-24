---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWNA7U2P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGB%2F2OKwMRQxd8DrIAZPYA%2BEgah8wP4k4o2nXoSSJdKpAiEAsPWNc5QyFNageNBmkGzYQFtIfvDdd%2FlO7nVjoKVryyUq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDBZkZPM%2BzcvTu%2FrVjCrcAwe3UGgABUH7M2BZyEkwfWakziHU5X0Vyuz3kFcWzpLUsRmHABE2ctKRqhxTr5dGTtd3lU4H16y0%2BpL%2F0j9XQcQ7L0%2FUQxyK01KbKmMXZ5dnKiYKCv95LyFdmwXW8%2BurluppSlMPWTLn9jOo5v7WEO1DJQ%2BpLPVYoTdO6hCPSWCWaiH%2BzU4xP3wehfpfSEhQj%2B%2BfpDrqbrad2FZ%2FmvveUaGhSsLx8FhjFSLbyE08mmoNKMnfSy9q%2BLOHcwhEgG3d2yWtOVK86%2BBbeQFupSNYA6OB8kgpvTl9CYdaKj28n0tIL%2BVQDHcWds316qdFDrIWb6R%2F0X18FHNAzzr2jBto4tuOOKUD4vy4qaciHds2%2FkyCp1uVV3T0LvwZM5nqCiay%2FW14%2FaYk7%2Bvua95fF1oCgKLbTRy6imddwylePZeQGDkEr%2FDwMaPUxRVAecMWMM9qpCTSSrzFTMMmbOrtsDHuU%2BAiigy3P7KrK6nSJqd315A3DwfEnVk21i%2FKviEqB15r4g3fbBGiLRgm4zPadgYo662gKQiY%2BqBavmJllBVJ6VP1O6iE46B0GUeT2zcczIKCemLYnt7st2oSS2F%2FRHG2C1%2FQZMuSMoAdyWVj37LnqUmeNZPg2YnfEVfGklg2MJWL7scGOqUB6osXwvAjLgYeUwH7lXHc1DCwLT2MiL8DtadJub4KYnZVcoF6MPOkI%2FU9QtemqntgW6mfwOtzysAkx7mgnd9QMkGVoM968hlFBlHiLj8Tvzpf1ApaXqb6564S1zuBvCo%2BjeBN%2FEYKjNVamhkY43CIScIL5ESPJkevHq9Yb8ZH%2FNkQjkWNuH2Lu%2BoJYBeMZ9Gf%2BqvFDXA1%2BO3Vk8Tbf3nqdemOL6me&X-Amz-Signature=f2c12aaf8be66c64d4e46882e629fbb1c72256c25da1b2ab6b8a77ddbc4f43ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

