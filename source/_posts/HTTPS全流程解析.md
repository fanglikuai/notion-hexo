---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDJ3MEJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGy7LYCTwggoD0KY7cF0wDd7%2BVgwzCqeVPCL9e8vynEdAiBRsD%2B7WS8P1xzWroY66i0Rqu79qIloH8UCKz8q%2F1I1iCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFhlv6EtaK3%2FDoCKOKtwD%2BxNgO2%2BZxwUm8ai5uvj14jSVT8S8ugelVCbveJj1DjMiX9TNiPzHkBuzai6ILHUxb7Um8JmAJgf6FHCDs2Khd9RW3J%2FFLpP6nndwQHe9CAkeM5OAi7McudXDwPVMQs%2Fz%2B46KSpeHAVU%2BPceX36TRm4Qt9f7mqjH55KeJql99slF2ogrplqEH4XMdJFVQ0u7WXg5y%2FKQMCDAhvpG9GWe2v5cXMgJyqD7SmS6eoje5gRIZNnxmKKTOC%2FkUkOWHx7HXp%2FM6eQebAKFYdP4Ilqg2AyFPGEnaCELUGEHSCvMnQCPcNTj%2Flx3ub1r1GOIArUe6HT%2BjWZ%2Bfw3uvISl5gIE6aJcFmweycPrbNcS10e83%2BVkULkGXzKFUUDTgkQEjIE%2B6RqX4PvgiGAqOJV1WzNkm9OL88iGiwdbvWVU3uAI7bIK6Hc0XbSIkxnCdiTKMljHhXxEEJo5jeWlrIGo5EeTy9exuwld7rEiSoC6UyhafrZY8p5mpK5gR9Vba2DqO4UYPWZyptgkFBgpeq2gYXRYpv7x9nabpyGAJUk42dF8TPNmLtHyWKhlbId%2FL5Imt4uK0EpYPC7geSmbAHzijTzS6e0XNiYZ3eHwV1WnH0M%2Bo0P8G4hRbiGUuDz02X98wwr%2FkxgY6pgFgvsO7B3sbN%2Fn09SgcYSQBZV96H4GMVqTDmGMtmeb6eJQ4MAlYluTf4nBgI0xSDsBPGM8Kdtk%2BwxKaMPfHP0biHTBAEnQKlqnnH28Irys1kHxRDc9HJAYvn9ZrWQnXnK8C4NwenBE0BQ%2FOexj0PdI6tLB1kwzupQZgivGtsmKVIUE5gLKAE8A%2F3WlQO02uS9oZADHZhophp5DABKQdWTHUySwbyONF&X-Amz-Signature=4eb34f4f4ac80de789568a9eb0a2612d5b6e41350c45624f74bc51e31f6b356a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

