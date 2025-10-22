---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667I64WFPZ%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIERG89VlIm%2FbUgFHj3trPdngItM821Q%2Bhn1mE%2FmOXEW7AiEA%2BtRIph3iAIFs9cwQf2w3w%2BpHJ5Do7gxKK%2Bk5aV34cIwq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPesoEnMtWc1RdawXyrcA3O26%2FCOqbET4qEf52aFkydXrIsN2Nykbz%2BR%2BIOcNMBi2TNZzcl0o1irp0GECUnN7zt1GwGm3V4kWAh9sKZT5XO5ep3xCeOPVZGIFNAGomSCI8BYp2iPP0Umjwj47u0JhMkBYM1P5QD4XDQdbdTofc3OnRAH%2BJpVF4eVy6cvzZzLLR5gRRjF01O5Rw%2FU%2BP0UtTilJ0oILYRMMwT15ngh8YYm6nLga%2BpM44IA3BYLrG9pCbCelN2Ro1ZqZ6OkhtA07ghqX5TRAEg8A4G5UwKqiZUnMU%2Bj7B8eocjikyp17kbV5SJhndcBzxuQddZRKWRzJ0VlPwcM2NAv0UUSSTVvnCXi4QNiobuRtzMxGTn7OV%2FM1dcaBe0YaYmEYQnIpGVvwU6WVWU8f6BsuEQXslPlk%2FfmchL1%2BTOPXSBz%2B%2BsaDU%2BYA2iWJ3O1f9dCd5siwZ6LkDavjqYgDqNv%2FFj9ur%2BImzKN5WuZmV3OMpWvN3lQSQSxUyTsiM93rV1J7Bh%2BCezNueAI%2BADbOFeTjk%2Bbfp2jna01%2BbaV1%2BFn9yc8FQRWjB%2F4xNcTibwxyeV3xGVM2nqpfADGyP6OihhcLbA51xoQUteoMJxlebKHv6hBzJ7XrKIXK9DoQ721fz69hljhMJq35McGOqUBjyYnkJukGB26JnnhenvkUj%2B8zNb4538tRBQl5fFoPgTzz0RfkXUChAFIZmRFAJVYrzCQ6jIFRpLb%2FxlgdaDnwgHm%2Bp29mezgMhohLm8uT2%2BOX5HQ4IZJwp%2FZof4ujOnb4uTk2sE%2FVJIWFO%2FEhlKcaN84W1SE6dEV5iXfHmznRXTrDhzGZtDhkMfrxCVT9pyqWlMWt1Uq9sJoG%2FG8DHoj5S5OVouO&X-Amz-Signature=0c43dbe69244454211f594656aa6a3d903b50ca2daf7152fcba0897e27d775ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

