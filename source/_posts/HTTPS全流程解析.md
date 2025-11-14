---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQXOBBEP%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEbzzkzRmZ6B7JBZNRYFZvpgYAIG%2FMVWfpLGGgO0V7wfAiAKoLb7FSQBmGH54CWCWLXHerdMKoZUJjjBv9tFD5ejVSr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMWtjThEPcLznwdC0JKtwDB8VmQ0AO%2F3NkHId4aEDii0aYKXuqMLcpbmigduWwk5SeK2Yd22zNOUbQV9FvlgVPnzU8l%2BpaaKjcLo3L5J4vNDCB6Qrv5Gm0j0FYqfUbKd3vHlNH3H5QRd3Vh7DrjLLzeA0LPMJwy8CJeIYtfx1WSiWOkwpyvvbRkoWLXt27q9F7hJKIIEk8rWhKaLHhVtvzgVQRrfPoU0KSQWLFlYZD33VcqnnWjQ9Lg%2BJaW9tVzLZsr2cZNjP8WCc72SXyjytruLu0wVkbzAPHeeBOIDa4mfSl4FrR6W%2B%2BIT0iWDWkv8Ug2zBye5fxKsW%2BsL8dW%2F5XP%2BuH%2FPwXWbbUK%2FUOeD6yFi7G9B%2BHCV7cOf%2FlhKHCxbbjbEaiH06SV31PQNzfTWi6Mcdpiv16nAE%2BBhrEPo%2BKVxDpFgw1gqQaKh7vZZ0xVWotdyot3WMnhbG8rTUr33QKUiG1HsTfG2UC1EUdKyj0%2BtXSTIXhzXbTRSNM%2B9Q%2FSBna8RjGbEASbSFPNafVQXf1b6veyib6AbA6UODUR%2BX43qT4poEXlVu6NSKto43zg3fWi7SOPGThmonmhd%2FlSmv6Z5%2FX%2F5scKUDoPTFw9HkQseUuUElnG4ja%2FhLEaR1R%2B3SQCy%2FAoNX5Vz1gkXMw4JHeyAY6pgFGXoLBEjNt%2BmThY%2BzTW3YRB1hvBUL4A%2F6iJEL5AifBKEkOIASTOhas0uQXJ3Rz8pGumVE8eRjahSxf4NJweuiL5OD6BwR6JGI%2FWg7ytWq8ARxLjN8RwKSCTK2Gio75iNHi5D6OZwq%2FyKjZHASSxZinwNDJcTMz5Gh0ytNSYgm1Umo2vuw%2Bo85Ju0QUtyeHVrqHryIp%2Bh318ulVNEqPq77HJYty6rzJ&X-Amz-Signature=05c0c86cb16e25bee59ec863f770f698b165ba04053c7826c929d39dca82addb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

