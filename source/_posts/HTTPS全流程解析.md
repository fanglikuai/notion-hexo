---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3J7NQX6%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUGI9eyciCX65Yo1hCDJcBq2FQ3txG53RgrVfBDA6a4gIhAJCE6dWtGX%2FuV4O1Lpu%2Bg63gMcHVp4adXCXOJCUx2gJhKogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzSo%2BM6imZgunhEvXoq3APezAYLfpRyirlQamFyLJkbWMKKih8EIiPzc42TxNB6IC2A1lMntK%2FG%2BuM0skzlzRQZP9%2BAWBZAntJ8BicR92n8vFJQOeQu0ve38ZbfH4t%2Bi7A6L2fJCRkRbcnqQv7jrrC3wbp1s6cu4wCpMvAZJGHOe%2Fm4swNNmPOOE1H57wS8uBiQJ4Av6o7O%2FFSGo0bP%2FXeGB1wkZCcN6JOe30clZI9pbRVAJIl1D5CIKAAYeEzTNPGxTMceq0CH4lQR9q1GTUvR8J8MRz9FNw4z%2B04QTBbVzhBrr8jkDDJ6uUqOPba2SVHoYv7QY%2BM3xqyIX8H59SzBvKY3iXKQGGVUK6WPd8aAzCM%2F8RVblpMDB%2B%2Bggr%2Bw2b%2BOY4hI8P9akuwRSs4W1%2B9X6sYlPUISiC2Lfsj1Izie%2BOKKtzepG9QqkSIUgnBP1%2FkTnck63SdalvPwFtTK2E%2Bkn3u%2BibsUm9K%2FCk0Lr%2BAIEKcAvl6L6uZfoY3O1qRR14qy1AJiwxk1cT5S1jPmIMqS5eHAwTTrHij%2B7od58lCFVOkAJ%2Fw62Qmi%2FBxnriDYVZwy8w8qid04va4l6LLsGaGe%2BkZdTdSsnx5yBuY%2BCqiJdQ%2BzgjBKPUd%2BJnyP2QAMpPeBN6IwJLrVOIroRTD8ouLIBjqkAYJsLkAywu0v81vf8V6RE8b1P3KnUmbMbx58ORvJqbIgjmGHE7d1TZI9s1KrdJra9JwqUS1CCHQncuVqPIqWAltRauh7bXrvzj3y84zE2WiNQ0fY9TYoxIKoO5jnd0QjonCDZwr6ft4dyh4UHxMgDz1Q3k1IeSTZSwztWVq5pGa6w0njAb9CXPXFMh%2B7LnN%2Fch9W8NWdjakgw9T6S%2Bd0Ql1to0UW&X-Amz-Signature=9e6c97cf512cecb5f7cdb5ee8d7de4f6eafff6198219a0c555e5cc20356a971a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

