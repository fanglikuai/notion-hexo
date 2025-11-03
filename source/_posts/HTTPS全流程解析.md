---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD5WR6KY%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHHxy1PzUqS%2F4hBNvSpSdpUMR7N3USlT73ieAM%2FAD4uvAiEA465k3sNB6wxOaCSbn3l0TGpaPbei688FlDAZrJPyoTIq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDFBByUxTnZHdsHC5OCrcA0ltRfosXiNirPzSWDu%2B%2Ba8vCxQVe7LPfVko7OWw9uQSyXCfZdXvv%2BxeRuByStPlhfiuOAFaTvcDQqxDqc%2FRk1O%2FLs7HsZmpXV2%2Br1glopqpgfyTK6Sru%2B85Y%2FvGoIcXNc6g6fAO6%2B6S5EsElDEH7n7dn7KupcDmgo%2BAJqwiFtqnRllGwUK6EmHth70118yeODcVc0dYmlarw7qE7UVNhXiMkZnOhlFvQPgzfma5yhliv6hVg2L3iZC8F%2FxAg7s10oqWUdf6rWMtlxoV0rxozjZ4jaYr3pYT2OvUtr18Rh8K5iM9xsyVwBz8uAvCSgWK3BBRu0eo12kGikdt9aIKveMk7CAeXuPn9q6F2MiQ6rZTOrWfzwSQJtalh9ZpezneA3MbTfijs0OmZ%2BXmoEEFnkTUmYWKhZEV6VaJhmxyXcD2AYW%2BSq1ioeeyjPmvd%2BDcMBdJrmkfDMPc4BwRC02CmSaXVVPVL7HlGxE5TxVpByWEEkbIKLZmV13DA6tupjhq0QbaV7df1xqc4NDaFjQSWMdU5EU7eZ1x6oi0QRl7Y%2FRiTQcDJDY729AIRgPO%2Bt3Iwzq8tsymkztOWcMywzEFHtSpu6JcQ8swrq60eK0klC3oE39yQyfjZmidtz3fMMnQo8gGOqUBiUpSomUNnK1Z%2Fp7HzATSEUl9mK4JisKmgK%2FJSTRqIQm1GricSKOmgoqmloecpT0fQIfACZF%2BZ1tKJUee5qlWAcrUH0Sx6TpK9fy%2BWsCtjhlEwVA3qcBFH2jieM2TNld147ulOkiqau4g%2BmuICnEJVYoIhSXEqh6Oa%2BVxWNGRptLvIP7%2BYR7bFIcAYOlJ4qWvArbnsjV%2FLPW7jmB2HFbyihnmGm45&X-Amz-Signature=37dbb7fed5141e7c1805cb31f56afa82936802046ba529262460f785f9141fce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

