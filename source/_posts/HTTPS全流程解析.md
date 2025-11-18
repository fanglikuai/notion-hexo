---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U56X22FW%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsnZornaDvtYZoOzn94XQ%2FAqU3gFKc5hggj0c9oNN4QAiEA%2FfC6uRE9GI%2FfhAW5q%2F4Tu7i7sZNx1xpXtsrObG1xQ9wqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKWCRHrooGfFeSlZ7CrcA1rk%2BUpD1Yv7hRruOXZBA3joRumhWH0j1K6hu0ph%2FOARUxW5cH92fOn8ypCIHH0tCWc0EbFSYZv%2Bq4qpxCprojT72jg2BGAc2uIQCsQN9dIfUwZoYaNpA8bj8VGmGw3nd4%2Fx01%2FEV8Q7n3aBm%2F0OWwTqJkCXt02x%2BJAtArWZ2BY39kMV8NV9gPH4Bj5gV4gb56E1rGQPqu5KpIniO0ibY1FLBsXkCTg%2BVSDlvZLfHhPIL1V0gduMickwCtjWA7u0e9qouKSEr8OV2IdL8QHT7ZQnqP%2FYk%2Bh%2BVpQAAQboZ9v905a1Btaw3wZabDd6YL8ZKs8OHKR5g1JRlVjIGGfs6x2mI91eb6NNLaOARf2Uc1xvEqu4FAXIO4jtb1lPrQ3gDKd0TiiBzf8ZmEBzqx7yXMujL1YkZRN3Dzzu1QpUxMqJbYBVYTb1DfrFVyptdk0PK3q9bw3njSf1vfcWqfkFq287WiRlSF%2FaD05iVyR0zFZD%2FihbQLQIy%2B1xHYRNKf1XlrnRxclQdzU8Dri8NR7PDqzTRDMrnXrUBQ77A%2FKdPNGP22kZF04v0u4Drxz9OIiujYBcSRh5qFTwFd%2BehgRDb%2B0HVP7MwmR%2BNfI%2FUuAnpu1jDgzKzPgVtbe1D16wMO3c7sgGOqUBcMnaKs%2BaOjWASNp7reLBLQ9zlvGKgdUlF9J4tUfF5WPpmSMHcGKmiWhrMQi2gXK3EnPzLq78i3ERy0XnvlAKxQaqb5IFGxD75D1kteuBQpthQ5BBn3TviSMf5CaghMDsYSyZdxt4LOD3dLahTYc9xs1fitq58whBEcZMkmNIaItmYYfseNTjYIuRCZoOzno6TQJWwljiylRia2JsmlTjaCMKNiCs&X-Amz-Signature=6bf02b7da6616c13b9b6c59bc7087eb79d9ca89cd05d3304305fb89f92016c9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

