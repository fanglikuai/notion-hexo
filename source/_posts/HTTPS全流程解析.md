---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666M52M2Q4%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIBYAurxaZ78pPTNmeqbJI5FO3ZUmNR8FNd0M1kFN4WZjAiEAmoKt%2F0i6Rbl7cwJVqQH15M7COZoeGHFi2wsyhcJPrOgqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEDDIP1x5nuqmqe4aCrcAx30llhPOvEloShbdLQxVXf8maBpUkmF52u8tDSWw%2FkN7SFVx0kxfs3FgVmQpwZLMVtPKuX4ZcfW6L%2FycGmr2%2FIq85hNcNdaVW7FS1%2B3bod4ufv4FbC3vwz6bcbZ9xDjf8Lra6wEw7VcbAfonm1Gq6F0SpwNI1vZ9rDCo7%2FSE5YHL%2BWiyHHCTdUwC1zz6sfClqKEq7DUxWde7pktG08lCirHgi8allA2TT%2BUwf29upqZ%2B7ASlQpijKR1SkZXwaPjRewdUIdLTm14vMuJ0VQR2sCOGpKBPiOZN5FKcZxtrWxOkbJb1LDc4y2i6%2F%2B8mHyk%2Fm%2BeWyw0kqH7l%2FT7Ru7JwwUlV54kfTvfi%2B4leMUr2COEZLpbKiB9KDEMySlJC9xmZMr5fSG%2BxPF9mTcN8OrozY3NYDCFea4FH4HL2XoTtbvk61XvhlTKR3mU9FqNx0uPwN7bHti1O4pblrKz2YYsmKaiGMc245cM64n4T%2Bxx32j7eL5gg0WJ01jUw1Jr%2Fwvpas4XBA4264CaZordFf%2BBPg58UiGbBonmIADXwmX1ByltWfzb0JRAYpbkQI2dviRbTidLbusTzhBEaXW4OKacPHOvuOspPw0elTYOHdM7a7hD4kTS4qrbckH1ui%2FyMNPcr8YGOqUBa7rinBfTio2HmgVilJYwXX1ekv4NwcH5HTr7SVTM5MdFbgOWqfp0drF9f8d2dO8qMQbI%2FRjFnvpQQNr6TQU8FQT2hJvAu1Xvfk3vjOyukx2DbmCBMddIvtxA3v1XOg03NIk%2FxDBnblgMqnCK35eAo3INU1pY89fMY7sR36TbfVtZgeYQGJ6ubbxnCPX749p%2Fr52hCpewqpKcwGoCmqmcX%2FIjOgAN&X-Amz-Signature=63f629ebc5ce6acc355dd4ea818861bf6eb4e125a1bfde877efe31f9d843b331&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

