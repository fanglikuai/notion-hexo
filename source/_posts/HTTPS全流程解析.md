---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662NBAIAH3%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICM6DwVY7S2ct6FeknhdnA%2BcdLJgIeQtei2TFdnf7ilsAiABi5FFiqBg4BZGG4aW36pUPVHpmghhMr7RhgdWn4gIXyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5WCj46lnq8eMwhz8KtwDEz8jQ%2BpWiNw2ccxS1LWsUtiGUgUeBwG7C4V21Nf4ySoBF3s%2BkI0D7MVx3F%2BApLxHuB68ZPWdrBYK23vir%2B0BMzIhIRGacFHL8t1TNKUnOlnNFJcQLKkAsIXWY1iRpixM6PIHfKTbboaAgyDnivtaeB8oSsa%2Bp14qYj7cVnyPr8Nowc1gOtrKNDn4r%2BohRhAryX5Hgx7UoFWtbI5dVMgnVyzDWFInWc5plfHcaQ7dHVlwzhjcEXZa0zMaOuyWD4BpLzjQPpn309quAyR42BPZugxVcyea%2BRvXuMZrDjtIKrIaEN4YoKOIa%2FGwlQowpMUrpDUjJlceTaT8uWff7OKbUSpamgm3fuLVKN8Uq0kDL4L52iN2h4FgWKr%2BVSGBiYu9f0H7hGBsesbEz%2Fdm6x%2F1GLczFgdpxnYVRxePnWQoYQ28pcQZgVXej0rx8Cm8gf8kA4J8AUm7cs%2BAi2%2B19BUc36OsSQEOqEF5sw6FF753HSaulHRULzjeK8W3nC3cha7YxJBIzNQv4JcPwTZlIxBizgfLLLTWMiDW%2BtkbAE0A8Rn7a3wbz3MMTOWPbSxCsNH%2FTQ%2Fqt9%2FrWu4k%2FHJ78IqFz8jSBBdznCOl3knQNgrYnZhurGd2jj9C6Kp4EHAwnrukyQY6pgFARS6jMyGd%2BRK%2BycrYtO6fQYuQjbI1vEoDQT%2BAUtg74W0hO8RFwrbo%2FnTiAAP0VX3nM28fEcYwxGBw51SDiIf02OTKM10yX75EJgeprNpI2NtOAP3M2hb0iGuEg1rb%2FlNo%2BP77mjYZ7lEhh5x0sEhSvZMxZg5oTUq60bLqZhXkSNvtQtUYI1m0B61XW2OeKz%2FMvkdQ6da7wyZRkSC1%2FycVS9o2vrgg&X-Amz-Signature=1364c6c0c7c2844e8e218e2e428a682191913b2fcb58e8af53657ec6a2b5642c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

