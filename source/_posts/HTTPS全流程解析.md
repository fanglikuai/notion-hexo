---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q65T7QQK%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T160054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEsRTfupXA2ToHU%2Byl3ezRbExnkrywn%2FU9u2ZmJ3CKeLAiEA9wIcJ5kHBiJdvUXEmtjt3B%2FluK%2BzgByM8D8eWckq0vwq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDHHqeDrkt6V4e0fuASrcA9AE0RaZ4FbcaIc3r%2F3Wpp8rWMd%2Fm8%2BDRYhIgsPz8q%2F8vq%2FtjvYZr0AaTiWfpLZ2EU6o%2BtrTKVso6cgmObJYlYEwn9cL4YxdNbP6R0c%2FcdA8VEHEUyqLQTkoBCOKXH9isd%2B7GnhWcBU7kgh1ipIQ%2FyOdvHGkvjGC8uB0aZgpJb%2FJ0GUeOcddjcucA56Y%2FQivkTByr7JmiBikIl1RHCUlLGVlJ0K0GmsFrs2zs%2F4%2FVechqa3kqQZXE0IHbOKceJGX8iVbVv03QlllIrVuQDjDW8sjO%2FfyAI%2FAHXp44s16EdWTeV4rO8%2Fe1sI0DLPTWvpv1W%2B0Sv6YjLeWFbD53%2FWYEJOhQYQ3J50P86315FvPPDaKu21FWkl1%2FZJzt4%2BMNx7IoBP2MnNX1LZm9ZwjHJaCrwkNhU3i6iu64ekgJGbH0dTZLPbxhuARX6QWe055aKkX6cluUctgvhNXjLQqd6aehAH5F615BWa48SeBP7WOS6yCKywOy3tYVlE6OXLk47h6dtjITqYIKjSSdrUD1f1FJd3of6W0jIxjj1oFl8jEXSgMasphxYVMXpjOpjHIvzZUZgzy%2FsJ3cgO5hsZz09hkb5kXSlbKOsNTdyW4abjxO%2BW6ON732U1FiGbSCgVpMNj6kckGOqUB2D9c5SGdDdF2te3I3ObsGZyN3Qdkk0mjRf0Caz6h32VKIVCnVrmFnIicRUurfFumTyyAN4nUKCvIJYYabN%2BIVf8Q21VW9g%2FObInT89DbPnwv8CL%2BWct9ViBvtLv6OWJMwWyb6FbKS2e99a7U1wVcYu%2Boj1N8Iol8tA6RoFucJTTkbR%2BTNu8q0Fwa3RiHbp%2FZcDpedsCarMIeuPzDq7xWYts3G1Or&X-Amz-Signature=8e76909b88beb6c198647f080c0ff0f798df410badad9415d920680b32bd6881&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

