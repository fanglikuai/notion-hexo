---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBA2HMEM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDRoHUgzXLWo8KGarvtmYoRvK3YDDLVjQEiYci%2FVdF%2BbgIhANlzljKLJmqaswTsAHTs0oMp5cKRCHEKll7kvzWWxt9yKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxWEDMAgFmfdmC6uuMq3ANYqW9xmg2ZhHfuHSBfarEJf7Lb5HNEnH%2F4syjDwvIrcgu7mCt8Fuq1PFHSxsvF1wJpeQMAQMRgqPcvFYOx4la02CR5sZVSz%2FYm3h7G%2BCQ%2FjT2JI7QpmE8nEdL%2BsXOQDFKJNgbIQas7ZGUQ2zNn%2F3uMKPqM%2BOY%2F8GoJwig9Q55rNVMRTkXrgajTffPESEENuLYHNyF4M%2FOBnRaDjpAEoDMEz5%2FRIFtvim08B8mfKPbYfdkU84CjmytowlIkf71%2BL8WY0i2S1Z4pIuCNYMsGNfdzf4nDQiDXTTTc%2FJikLXCLwwYM45vlja3%2B0%2BdU5BPTcyZb7OyhNKk2juml5%2FkxceUAkAgP8PmQkmx5R3YdRHjzzaG4A0fZw%2BcXLyWOmolHgapeJRdjrWqpGyRizZRBTg4q2ukBgFhh9WCRwMX7i0I3y7Tt77%2FIAy%2BZQ7ebHXDkY45WonehiMSGCS9wf%2B0LHRFlSMg%2FOHgG8pOr1uQYfpr4umGZ5pka1W6Yt3%2BvPCOqfCwkerbHOfruMlgvTePz8us%2FzNYTzdsjQjf09yOt1eB%2BlYocFmAszIMeULixTQdIHtkTQnJ6udKK0%2BezrzS9jDRyqpPIUY76M1m3JwZPvZ%2BARofAD7Ejdccnm35hwzCq1%2BXGBjqkAY%2BwHjt%2BLvu2IDC9xiiuolsHYz9G9hZJRiVLVhWgoikuWcbA1Oj6hlVlddfESPLwJbj0m9doPlr1KKmvsdCEInjgj3QPN2yIZJG9gXLoNb0JDQ%2F%2FsIX%2BnlxMGDzavFSHdIloY%2FgOTtm7WBL0BFcobMLlQWWWsgiVfQfw0jjqz%2FtNaHSoWP01wJ3quLK8XLt3Gh18ZT5YEfivA7sYZXrghF3FM6Up&X-Amz-Signature=5a8da5ffcda309fc9eb6f4baf8ce6895e1d07be261107a194608a479a0bbbaf5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

