---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636RHJIT4%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIBcLFJiMC4fiH%2FOKCgXc8Bm8Hycm2a%2FjS3AaOmMHr6JAAiEAngnrrbPJoVglCNPZnSwFi1vO0tgLIsEbhSnj59x2KzMqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHDRiiEc8GwWJ9RCJyrcA%2BnsK1PhLwNNfz3JXBUi1Evj7pe%2BgfBRCQexEyjOlm08MixPAuJXiGks3HqgUiGkEIvCTZhpfVkG9yaXoE7qmUXJZNTlnwh%2Ffy%2BnCi1plIJ38NDa4maLub5Oqw3YEgSmuc1oJFGqFjvH45%2FacGify2AjRdEhFWTzBwq%2Bsc3tfo3cu0K4NQk0UpgQfDC4BHBmt10OI2fqxo%2FRmtzwg0P5ngJbZkOe5bo%2FoJxotO70zuumDYD7U6iXES0bBLQsWMU6%2Fav%2BBTVxmulZegmk5QMwR6d%2BnCC1LK4jplr58RcPwk2v4AxeBUUL90zbJ4nAKajRZa3tsActH9zMpO%2BEa6oaOO%2F2%2FBMd%2FclW4D4SoZ9c%2BYp2QxdGHQbq5JPlFq3VIHjV9%2BqIIVQ69i3cvSnZWs%2B8q6u4JToElr5pny4leywGjDqJ%2BU3o6ZeLvRgReC4FD4gwYENbX5%2BtqzJ%2BO6KJ90zb99KC%2BNYy2%2FL5E8Dvh%2FnQb%2BtYqeRntLaLR4hngUawDTUHe28t%2FubrJFfBHAVSSBpm%2Fzn3yNKQHIQ10p84EgaxUlDI%2BdBzZpxjyEw%2F6Y%2FRmc%2F2oq5NDwCHKsNIIPEEyTWTXXzvcEYmfN%2BkKtCJqYb93G3m3PYxIjMmyKGtW3iAMIrqmMcGOqUBYdgmvHgXRL7LFUUX2bAPrAwL51C%2FxFbID1%2B16UYYiDD2xcxWeAzRwI6z07C%2FouhDgItXABVzrBkAvENgNVQNmnDWtuEh6f9PLLMazhCSXfHy%2FqCluim%2BXkDupZ1Fk4m8hG3eWBQfUrLDBgRmVHvzVXDIWwYEADJAl7%2B4lDXyFPckEgFnq7cHjPUEBYqwlxRKVyVM9Oja3%2FNDmRyR0g7F3x88pR5%2F&X-Amz-Signature=b01daac7deec297d65c88785df5fc5e9720c381fd9f291bb4fa9f89a1984b8ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

