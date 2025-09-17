---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTEZU7V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIEFuJoTX%2FypmiXP1pwaqZTQ1HyYa40jHuz84GN4nnsY9AiEAqn87kikh6Mfm%2B4c7lxHQHe9UN2%2BDZ%2BP6t3tho7NwoDUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaI%2BvJY%2FJxQA2fcZSrcA9iguulaXp6eoAr2MLAkGT7ykCveosTUpmd8qsUGVOXoNX9BdGM%2FfJ%2BN5yF%2BYLFNx1gBFTbuHMkmnTUUXCuHh%2Fe5LJGCVUe86GXXSa0YrVhQQkkerWXjmZ4pTPD0XlAIcoTKjqM2YXB2Z4NXfGX7UTMBjEurF%2FRr3oMjrHxSMxYOZLWqyHmy2m73T8n7U%2Fs%2BerAO6oT4cs5j0oXIt5Se%2B41StLOQ4wqdj5EsZDrohGL4hsohVXu%2FiaOD0brL2%2FcLNpz4ksrOB5YLwCS5nJCJ3A4uxg9w5jBR6zHFLiEtuOOK4yZOF8vwyYfikD4JSdJAu5ne8L%2FdIt20KYPMkChTV6V3jirntADCTz5bauupxy9X5h9hzQulNADK7KFKjP3LX4xsUy8OJgP8dgw%2FMTI1%2FhHxQw%2BgqKOwciQOghbA%2FKyE8Z7S%2BmeBFGnZf8g69%2F5SzWVR%2Fug577pUPUxkwvAI%2Fd7LUgSVgu9LEKE2JLJqrqygt%2Bx1GLFmVwHIYuoeGeT4b0bVNh%2BXJzfu0FflRuhQYrXrY%2B6shCk8aDAAKbjJp4FLGy1rpJkFAXitVeRtqWsqXM2LADa1rq3VLm%2F4kyMn%2FIn4X5lOSmhy2Qsh1HfNDSrn2tFDui0RYQFCEtBhMPrUq8YGOqUBwDCA3q4yhj3CxIkXm%2BbgwROpBCliiKOMRKzB8bnCwJpVGuAMCXndn%2Fq3DyktA91zY2HBCXw5SlDcKGb88CXaKF1CpSCjKG9ATj9dR1CzQ%2F2chAmDtBUfAZtceHcFE%2BEMPGMX0VB0SWxNOfhgLXgGg8yB67B1PpQcSncnnlwznLPeO6TgqcmX3%2Bk42pYgesQW%2FgqVFiB14LqU0HubpyJXLo4UYNPP&X-Amz-Signature=7e80d7d07aeae1ed2f70f6b03eeb90917886cfbb92677039f0b6048b420629a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

