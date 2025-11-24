---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q62CNWDT%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHHH7yxhjL9kOZOS%2BOjzDUN2KSOSjggzUEaCi6OC%2BuKdAiAOH68d1l7UwBjB1Kdd2wxvN4zS8rCg4M%2BBuvSVYMGODyr%2FAwhXEAAaDDYzNzQyMzE4MzgwNSIMg0CWq%2FVC%2BzHQ8vrIKtwDKrK%2Fe7eaPxQQWYVslNebmK3klJkAyASv%2BPDCNxuDRcfQGcbW4jYzL%2BxiD%2FnkK0Abouu%2FHfkDhV%2FJV%2Ffc7d%2FfBqrKhcctSa5AOgABVx1qnLh3hyuRAa1Ls63FczGpNYNMZMQuhzJc6tP%2BjOrzvny3usZSPtFU8OUR%2BbJTnz%2BqUO7gq0B72%2B5gTxmdIpWMHl%2B9bbNZLBBXm2Hhg6UstJNhPN7mz0vZc7xR8MABvmNLt7ArmZFH5nXndwEetzNjDInYGncZpK4X1Fw%2F%2BQ4g5WJk93KFjJBW0bYJEa4%2BLEMks0JFBnkfJwdnlqg27i%2Fps0uoSqb%2Bhll6AQ0v667VBIPqCOSRuaCxS%2FM0%2B81kdk2q937q8Mva7APomq1AwgCNi7S6%2B%2F%2FoqUTl9H6EHAS8xNLmFrDJ6%2B13OQukNSLQKzOYTtOGOM7YQJvsQGoZGnkyuWf9kRsjUaKdELlrbaq67BQnl5HdeMNkU0p5UIcGO7mfkwrPmjkVcU8eO%2B5q2dtgy%2FqITiDZQ0nK0hm6J1cZvhXc31kjjQf7Q7duajZYWBAy%2FrH0rCEIZga3nhTS419n6QNJsyL%2FJviUmYEBV0a356K4zZnpbmKhYvIw%2BAsM9lUg%2F%2BAhyjeQCV%2BpTEOXUrIwvr6RyQY6pgHI0S%2Bt%2BTNYwrLO2BakGF2pFbLnfLvi91pZUQWeNUqYAmF1O3Ts%2BbWlQGwdF%2BJi9JEhD97KLHz6haMQBihA4lDSzL%2FVF70AtsmEMsChs9PwSeeRrWEXLddQ763rmUDkCxMBNfFRIktcm9q0An2XfrQJ4FXhFO40hJ2CftZ55%2FGdjZmGkmcVJSD4J%2B6P4qnlu7RatzPzQA9jguyflGYYePVYHQKD81sm&X-Amz-Signature=d3a7fd34c13bfcdad3c07cc5c91f262b38645d9400800431753586ad9a2d45fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

