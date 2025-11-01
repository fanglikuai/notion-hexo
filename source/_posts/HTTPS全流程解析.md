---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEGIDGXG%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T140101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC5Yeoi4k5V1m844BSJenzT0llBVTMMZjmukZqniObWEQIgHAoCtxGMidnLETBXkBLkBi5E%2BQmwGUV7IpHA7H6Q1kYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAeEHxmwRllSVa%2FZbircA0IoEmM33UMP1pK7c%2BkFXuxR06J4yhXgxc4LeRq%2F46bOzJFEJUbWE0qVV10cgsJ0QI6GqvQbQqRNCVgFIRUS%2FpLfJzok8yuxVjjDNuVTSSeMhp0ctvHZQslv9r1CraF7EkzcR57IHG81LimGoVty8NMqoyiTlvxxmymByzUOqG6nHZDiWRY8eVPPWpQofwHZgVEDx5N1jlyraceyLHAqMwnJ48BoKiBlUqOA4pwlxhUwC8qdHSURZmURP%2BcQQ%2BZik%2BYRvhQxfzLsQVaMLEL29Ykp%2BgQMwX9extYTxaN6rmJcavTrIFTM0GNI0egJLRA1b7R1qnk7N4AjaLmkgY1GpJN2Ijn5Z%2BWixFTO4tOjfF17usaEv%2BhonU1%2BRe%2BzvVrSLTO3OIredU8g0dZ1397o8F%2FNWjJ5A6Mx04feXMsdcvEd3FDMkHDUKinYnB82dWogcwS2CZV4P3BSDgGhj3CobOnKYmly11yuI7oX%2BBxTLrm%2FuxIOQJBzJJkQwuyyDY543RguIOHi35LTh8TRNYedBMKNEc5RELxYLMfy4pCAzXBV6zhyNkSrqSQNXpm2wIUsDZaHz%2FZ3QJVt1tHmlCfvGGySu%2FDV9QFwnrH4Ud%2BjVlY8GLHObeFu7lR4utIbMJj2l8gGOqUBGeh54rigMc7wkMoDUgA5QFka1BgpLrKV64JVJ55hZ2VgiNqfG1T7FeDjGFcKxdo53Hr1ir0ut1aBWIvG1AdBMBWJPSkYB4bO%2Bdox9J7sDFsE5lFC62ziOaUSrEB5uIxPhLpJVwn1HRhOmoo8QcomYsnt95sSGxQJQ9ZDj5hjggQZ9UTm6PLnI%2FeXV4y1k4y4%2BqE4ek1ySMoMJ00%2BuLB8WE3pbdPL&X-Amz-Signature=1dabd483dd3a66137106de90c8e775822bb9e2751f87c609c0c1da91abc118eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

