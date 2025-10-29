---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTA2BVH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCID2E%2BPGBp4Ltu0upM6Dc4%2BFTPZY4njMITtaYBtjh7VbmAiAh3wPG0UcJpIelp0YAiLGxT2z57s2YUUzg%2B%2BNpZKRn2SqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCbayZkj0RbWWOX3CKtwDzLSZ2KjmKZrwgTbZjTNUB%2Fts1eWuEx%2F%2BWg9HP%2FWIF4Hf%2FnCOdP3KBr3coUe867zdM7v5mVTATNwYgdEs1uli2IxL9vc46zQYNcHLn4%2BOVonru06OkibYH0qGANKn5MRBCXGwx4BnQlh96zL8kiW45bFbh1uzsbfIvt%2FvkEPqN%2B4A2Os%2B1Au6PlbodvdTVJmsGN3sVV5JlcJrZkJea0yI9LvyaJtSyYaKCMElswcRwi1L%2F6C4ATsb8s7mHEkQQ6posgqiS%2Fve%2Fb1zLnVA828moLKHzI2qU%2FiHUSEbNUwaCoH15GbPc3r7GX1qoNTXfvgDGhK8cqaELh3DQs2My74wBAKgE2Ss%2BVo8aR%2F3o%2FliRpfJIYhzPPOoYHjNBCgWI0RXFu1FNQ8VoYUjV7wXiiB0oDrqF55m6pEe0zR6%2BeTeoW2t16jl%2FpR0csGxjkESwWelU%2Bqgwv3Q7VimjtljSAo1eE3%2BDEzo5zqXaBEgLuwFPfflC0Y4Kl6T4f15l8iiYO2c%2BXAyYEckYU%2FlPHc771uuzV%2BQoGyw6yQh%2FJ65fqAsgcuPWqDn%2FPU7gNcPBYF4P3SEWPT0dGs6TxnxlAhzn4ZDWptwT1Mj5i7%2BssXlJ0vU78jfwC9js5AzOcl7ke4wvqyHyAY6pgHDNltgqBRLwe3r58n93ssyHrUf70vHkW6DighKtVzO%2B9Qwx%2FoRU9OjzV3Lp8wUviLHRyOOzOzp7I38pWBFK7bmSREJc4%2FvedsHF0l3%2FWg1O1nEdJQygzv4IfVKJ81WidGLIVEItHl7q8U9RbnxkR9YBbFlwzmiK2dIIuz4V5IMak36kUC5hEknX3%2FrZeHKue86bXTHrICluUSCUmDlq7szQ1ARIxXa&X-Amz-Signature=a9de8837f72821c7f6069bfd54bd1061992b0ae9200835dedcb2df08e2138a93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

