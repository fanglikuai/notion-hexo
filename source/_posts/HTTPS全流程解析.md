---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GC52MUV%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T000035Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQu8Jg%2FlXgAz%2FUoZ%2BuxPiPixWlnmw2oAKOzWwhFbyypQIgWPpONnb8ibodOqfy3WJMZkO1OvTsRUJBfMcfHVoIY4IqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh8GXFaMbDic%2BepKCrcAxjTgYgOgEout4O6AcB8FpdsRotfZbqIgAWRp%2FPfW98q0l8yfM0JLVe%2BUImyfPkYJCBMilnaoAJMHdrRBNXtGvGmLlh0JQcPJgukkg27CdChKpKqhoAMDE%2Fpjswij5TPmXp%2B6VLdQffIG%2B%2Fz7Nok27SmzlF7F6NwLEZ33f0ZBZNDeDEliMbnVHbWz85V7TX8v6ZExQjIUWEP7Y7BbDxLM17KN6A37k2W4u%2BffNio4oHgrmHzVYMwvX6bT4cFfbEOAYvBreQYh9Ps%2By%2BFx5p7y4dekj0esVsHJPuDw54ZEQxH2nsJz4T2J0UzAz18TzaEq4iveUVPTCcK%2Bmyc7LwYZfBcrYhoU%2BFa0O4DI7oTdaQX9gUUOi1CaHUHB7yn%2Bl9vreUAWVlwslBDkSoqZP5AgZQ9OZemB9VnUI3y4aYbUEt2eqrkPyqDmGvkKZtWC7TaXNbhvpbgvqd7cVNkMIGk9TVaL1qX5heh5zgq8sKK3Bm51uqcMhH21jprXRtTdfAhhA130KLjHNqBgCkdT5qsM0lkF6XIzCXM%2F6XOTyLRl4tfZXUT2%2FmyE%2BFJpadL66%2FOvIhNVb1QYrxCOKzJR3xo1yD6cYcfZcrzV11muvequqVsxtdu3v5I5aWDMcLbMIb%2Fi8cGOqUBxizIpSa3YNP3n7dqJVV7xTWdOPcGmBWA%2FZHAPFRARDROaZvx%2Fo%2FfXUH87LwZZmyxwp7qSAVXWTpG6zBNv7p67UCBENTzL78INTEGQH73vaZFGd22nRdG57%2FDD12zxYxDXHH1uJeNmft4z%2FcvFf9i6O5Hq0jg3rBarYJVBen3P0jKK4ixSaztQLQanWG5d8MJqeu4YLi5N9ZT9juLJH%2FoS4%2B8AFBJ&X-Amz-Signature=85e1d3ea34574362432401663e8662c3ace899062fe512e0d8f1e3576ac1f4b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

