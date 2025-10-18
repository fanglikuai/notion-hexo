---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDGBXGG3%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIAdd94TbXSBiglM6d7C%2FmY9vs6RJTvouwIxhtpHQHL%2BjAiBaSMs8X7S2NHvAo5o%2FQr7fAlmuBsau1xT4HQomIEIIqSqIBAiy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRksZ%2BWvKy5KZoh3KtwDG1xXxxc61GY4uIvsRvcty5mYNy537IaCmmEqZVjJbL4y3ZPuri3DS6w11MTWyp6FTvGLYaEEq1vXaDLU1IlraeypkooccdR1FkE5LYl6gpyU%2BacOwnU%2FuVkB17beQ9IJ0W5WzK0Mcrp7aM%2FIvkwbQ7o8y3TMesqhljrq2OqhCtVTh%2BmK6inXioCGyGAW6BkSzakIXDk7Y%2FOJwaek7HjUzque8l%2FaFUBfYBUGZmZElZ3NsrDIyiGvgk7Fcoc2z%2BfPtHh39pdjYFZ2%2FEw27Sg07iMz3gOy%2FJgH22PvPmW6Vd5rc14mwS3sKNp0JIpTdzNzDMFQvDKiez0NVH3EqRDDgHK4mAYpzYArLEHHEVvvTBNbNnN01DHtqJ3XC2ldO%2Ba9Sd9yF57jRS%2BMOiq2ZzFCBJoiOcC1Vpbbvy6rcxD4J5P3NYlFr7ScWlhMwUtQXv9oNP7SFKdSzx8IPlKNm%2FJycMNjtEquEl4kZgKcWXeHQMO0Grp6zA2hy%2Brfqoyuk9%2FLZh6ntByEiwZvaA2byMVklWs8blnl%2BQkaJHdr%2FdhJVOEcPIdndivk1SytBjKZIHnSzbxyZ8xvITBhwrHP6ie2W2exZ9iEh2yclm3bDD0bBm9FHm6brf5uYnUyIOcwq7%2FLxwY6pgEorSd9UOELMDqm%2FIStUpYOypnsBewHvWiIHyOHirxrUq0vpyL9Nzn9fWXVm4ahTBiu2n7r%2FEuDLKxGw3h0JBYDYKPur9Rx7tE4heLMLYtuT4mj1qrf1gW%2FbeK4lWir1qOmuLM5FK%2Flz%2FivfpTXCSVhNnbPWZpCf92hv%2BHMpvkaXPQZGSz%2Fd%2BKHilUiau%2B%2FZBSgup9tHdwk74%2FnWbYJNez%2FELLwXgA3&X-Amz-Signature=6f95850b7ef6e81674cc85c00b29550cd457f933a1567037b5484e4547adec43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

