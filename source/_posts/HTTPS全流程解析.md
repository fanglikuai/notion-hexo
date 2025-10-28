---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG7ZE6LT%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDMvqZeBpApyOUhgRIwAvQzlB93TyvbnfXSDzzcwImiNgIgVbLEvyAD1Liydz3zewk1qAPDDJowHNWli9I89zx6X6kqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHcJvSNyDObKklb8bCrcA07T7HBVdsmYFe1Rqo%2Fsk%2BFNGGHRuHnNfPFtMaMVcqYXW9%2BTMKFAcJyxVOcoHSchUN7UAlKvibCbu87sINWqGleIuzKc8RJLS%2B7Yadprk9ewkhRR%2FzFoZkOqqhqcd7iRZXwRRzoyH5jfhQurclYt6c2Tz0H1TkpnblxArG454RT%2BlvyMCkJ%2FVdrvwzEcBWvUDG78HT9hDBGjoASv3j1pyhbotADr8vCWtsyrhykEQOCuaV37NJz3F1Ews8bqrwEN7g9PhVIzaD80yRoJZlv2N9emPWGwMGP%2Bjf6HPunrHFBUykzMjJVoO5UEscouU8lkkbWBRtZxUEMLVnG1Ug8PXTZ%2F81wEwcZkLYaceBCB%2FSuAzMYx%2Fm0o2F4K%2BVPkvfCIDJC%2FkcyM9pHjpZs2GXQn3HUEwCXUPX9lwJLjnacz1admPzz3VwFkbhCZKVCCTCkAFq7oFfkRJsoPk4fwJ24iHDZD7njrzWqfLm15RSigRhZZ0V1eS9dEWGKs9H3X3wwigT5cqKUnj5%2FNTHKRuLMHbokIBu400kJ0AqOQsl1%2FkURhiM8CORNSG2qnmkcP9xRhUe5B9VxBywYZ8yOBMLv%2BR%2Bmnr5S2K3LeS5RtFT%2B1ZrkfXSHKBL8VSUlbg16WMIONgcgGOqUBA91r7ehgrWsIjUACo%2B5YWMTJIygV8XyUz0Y5RJAg%2Bgm4wOw6gCytvMtFXIDPotrS9jsqbHNL9O47lCHRA1Dl4421zrHxSDuiVTcgPhTmwMl98bmJkvu%2FOAtDYQ%2BjSxN4LtjuYhq3DBuHujvTorGoGhs0CDt7EK8IeSjMG6hGIDH8fEz%2Fe4UTEY67%2BxOBZtk2Piuczl6qZ1UQ6QTTHVr4wq8%2BREm0&X-Amz-Signature=7e0127125ce9edc5230dc7e64b1536983e5e53b088ff4b3977bdd255cf51782e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

