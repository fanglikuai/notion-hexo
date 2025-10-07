---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MQ5DZ2H%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDcPAq0LOYPOF%2Frr9pZ7u5oIxGeMFe%2BWYqQHCC1e6YP7gIhAMg8XODWRlMFfbPQqanOO3NXbAmUModGi%2Fzt1QS1%2BkEkKogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzAb73j6ijMhu45B0kq3AN0cIlzftYrSKbUFlAZVvEKLgeBn2dn8DQCatlMhKHZQk1W67hcN5x%2Bh6Kf8K6sOaev0NtQ0WqqdLgUbKwtQDl97N2e4RWtBzYqaR0%2BBMcfU%2BtONspObmyOF8%2FmFnHyayEDQOswsGs%2BTybszXaKCSItel4q%2BOAdn7FjOw9BZOOnp7oRA7CgQJo27wC8ZXfgVxtln1k4ChlX5bXrxj%2BZJsZ1hm3jTkxFLgveqd8D%2FopC5F3DG%2BPlnOHM51PHUXibCs5rguaEFgcyQLb1WwUYwmqTby1VL0s7VKQX7ZeAWCZhi8TzVIzoTPDI9IRGZVi19L7fEy30DK9nOG%2Fl5t1ijQWw4bs388x8VYwLBtM8dUT73VWQidPNy3caMiqzNNyXJxB4MyFYBSVE4kXjMCPQQiy52Rqab%2FQKwutKXooWfEdVIKAhTLu8AVyAO%2BfccLsiPcIc59dHivuyNbOVkWTorvKb%2F4zu%2FEGOMOt1%2BCTYx1MHKr9%2ByRiP17nTH1wbvpHpGtOtt8ETusjDGJ9nNBfbEuNcnESALhzJLeFQ44Ee8N2Cm3ner9O4kpaZW5IYfqHz13R8JYjgEhQybvIDuXlntV6ew74mrAuzYHXo7rnFFN6u9mAgvsiqLrb4CYLCmTCpspLHBjqkARwfpPT7X2lWosEhm6E2TvcnOo9hhS8dxoyFxrDfpQ4NfGPJ2qyhoz9Ok%2BRZ%2F6I2Zh%2B6Jut77MyVdqaQEbAko89DkQ0A41HHcLvJuMU1HHORe%2Fa2g8AVw6ALEwWhsKuyoSGwPowowzVM0wcPfUEWfZ77vySnDcpHXRYZfeMbAMA2uiVauA7xdDhtz59fJVPcUpufd1ze4JK9u2ED799wREGxkLG3&X-Amz-Signature=44d21dce49d1333d8efc46a60101ec52582c9e7addfc8c946bd7f7716522fc6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

