---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2R4LNKY%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T170053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIAivNQUosGAaMF4jpd7ZzC21gPho3sUqObZq%2FCbY2ft9AiEA%2BDyHW%2FkXEO68CUWkoI%2B06ruzDqiGZzJ1CbwjbSe4PdcqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvJ%2FQ6s5C%2FMm0JMzircA2VyGzzY4i7sfmtXGfqxE1%2BO7A9BCfa3VJNd8fN9iD4XzzNXjN2VfjSmB42%2Br9IbwJ5weNMREUotmM%2FA4mXH4YoY0MOZjSULpid6ZVgI%2FogrJjNADqm%2Fzk5rV3%2BkKvqR7o%2BOvhEPFt2pEnGwuvPdTnwbhgOLXm7esJJya%2FyC%2FstjkVTE9184191hQYe01zuajtjy5CrFtrAHy%2B92q%2BNZIOJClUU%2FEhSVuT8Buk6DKfNg8TKeNmflR5kdrsAjlF7nbs7%2Fa9gbKoFSO%2B9R%2BD0g0MfbTeBH3QkopyLjsWh0oxLJrTjGScAbWsngiu2%2BeVy9iRpZojg1BJnjxgHEeK9EWUqJa7iwgUZEKEJG%2FrR8jgD26t7Cywat0A%2BJqhfdes4xWEG1UrLSqv%2BA6m4Eq3t1lRywzj6G6P3cmSEK85DuRiuhMz72inFcFNe%2FTnFHTjUwoqUHmJWibFVHEj8jxZ%2FgYLOte15d2lSApvjQCN2Ds1ZGPYF9TPVndwiuqozDTF00ThxXBBI7j8rk2lf8GVT8kE%2B8mx2Lr9sarMnkwsfHYPV8FoYKGjxqPlfUTrKy9nH7FMwrOxBDPTBaH%2F7myTj%2BJLL7WII7VSIQu%2Bso7didUV9stcMp7zIsb3Fjxj9yMLj2iMgGOqUB2a7uwjmnyn%2Fg%2FZIl0%2F4TpXp5vmwVTXNAiLjVxrJCiMlCn7OwdID8NITx7cBAkBCM3NnhqoPi%2BjC%2BPPvgE%2BBPBMxvXH%2Fk5x%2FRvNoInoOUVa%2Ftb%2BiW%2F6tl4zIf4dsfXbiq%2Ftz7wMC%2F6kvhQ3yi75ATQ9hYq4WRpd7gZgroSEvoWOQ2%2FRCECms3O0EyDUJK4m2EhMbeljoqsstf%2B1dQg959SId8TWUe&X-Amz-Signature=494600e813a94f30ca6186a7069c5fcc28fcc82d2483afd8796d5a94bd1d91c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

