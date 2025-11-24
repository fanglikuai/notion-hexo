---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2A6VIL2%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEOT%2F3DFFVPfehQjbehqAImy3lXA96%2BIg%2Fo5v809fT6RAiBxST1Qc%2FYfrJtPU1ZVnm54AapD1L4cO39Nc%2BXC1EsRiir%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMJDOqndOVNNXTBZYKKtwDuteArNyQp1HqY6%2BVziO6l%2Bu725DqQcfW00GOHfNdviqmOmb9ItkzNNipelPRHwWBCyLi1P8jOMgaLw%2B0Dt4MRKJRtPFV7oDynuDV5E0qQAdT0sBgNNRmP%2FkpkNnoJKCCbvHpW0cotnk4DNLeFA52HOBh64IrBb%2FnM8GE%2F19gK%2BMVByIh6TvQ1A%2F6o0DLH7oE%2BUTt10TxJJOEijsaQ38hNsB%2B%2FKkbZD88M3HeVmetodLClEVuByuMYbgo0wwK%2FuFMxyl7wJpLfwE8WG4peCPKL7JewE1WHwhwZUmQf8%2FxLtmGWMBkY2L0HN%2FmjD%2BoomHyxyakyXSrOiTefjtGW%2B9Sr61tRJlHE%2F%2F4Gtxdb9A5%2FY7M7EwxG4%2Bc5mRKf8MbIVXyF7aN1VqiOLonoEnA%2B5HvmfkJlSTwfmfvoK5ie3A%2FFEVbL7rUfEtrQDw0onJE37shqCCxs61gw450l%2BjnrVTviZiSPxaOEcc%2Bpb3nbCusG7qiNCtXk4ne6zMFEZEUKvD1awLKwN8AlhfzuuOJKfcgaP8XvycmGDsXuYJMBdGCO7UpiajjDCmHjNbQDFZuu9BQnmm%2FXtBfpturGr8KWSuOFaNhW9Va65MzVegciOOJGEp2MyvXocgaSTG9liIw97STyQY6pgE9YIuETKfi3o7WeCMDDJWv0RR42%2BITV7tOmhlkPF8X0d9YhgmVFgwd%2FYaRfdS28i3mF84zMf6nMY0dVV9SClncskBYudi2EpmqcTDK3Fnop3LVgcrS7Lev2v%2FRmbLPpTHc2Pze5x%2FMn9zNXyYzgLBnBBugXSbIOA%2BcParIWVB399EX5NU2gpXw827YOSlzuW%2FYcMil3ZZ6LIm6wg8dk59Bzj86nWU3&X-Amz-Signature=b5a2946982e760adf799abe01845f8f66b5b44b1a3dd337be100ef3aabebe001&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

