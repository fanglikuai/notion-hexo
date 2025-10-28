---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZYHZNA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T200051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCCRO%2BZ0rT46nhxDz6slY0jbnlku27tFB0DnaoFbzfWQgIgDF4OGfO75VS%2Bs5y0kEMcvtWVkbVsS4Z4hdCksayM2nEqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHwapwefq2kg737iYCrcA9Lx6KJRsJdelgcqNXc5pJs42uwkd%2FDtmvkSrl0CQmex%2FmtM6N3QDQvI6RxqR%2BzC6A6ooTqY506ApK7rX1JocI2RblQSia3W2bc6%2FE9x5PnWGORIHTaVOz9e1zfkPTQI3ThXzri85IdpAYLkZmH5%2BBgO6IZJSWZmiTPUyv4L4ToPseBAjbuGN1C9TlZHcziKhB5OQ1THr9m5EkERWzDKhFEmLLObU5FUTARo%2BdIJlOwede5QXCslla7cdcv3DHZ4M%2FiksN5EVxiuUTHhQaON4uQ7XFHIKoxjnUMgzfbMkM3idZowOu7F7n3My%2B7xOsrhqcog6FyLQ1F4QFGZt6sp%2Fy9HvNjOdXdXU32h6tfmFa031pMk4pkhaIIq63Jh%2FDb7tPLxonVPammlJVCY5d%2BeN%2B8nIHqk9qOK%2FnQzNfC9V6O28XBxVhYtmcKex4ZjFZHm4AlaBx4mZwm6BbmGb8%2FqS9TMNDhvRQ29rRNHZ%2BQVmY%2Bybom4IwpZOp%2FYyFKYMPOOiPv10df87HpRmJrX7LB80GfRFEqYETqQORxxRC94pkqFAwQ1gGWuaZlEuNY%2BLjRqq%2BMs4pfNKlwNwL0kn2GHsx8DrQuft6XLaTULdDotT%2Ft%2BYc3B78urDclmo%2FWqMPC5hMgGOqUBGCH7Tcq27RLL2LSUiWSCkU5Dbfxvwn3h4YRd4wJab%2BWDtOqaE%2Bi1PpWf9%2FKYNLH9RyY8cy0N3HI3erEqjARoWcml%2FZpRlnVyzjWqZtr7BYJ%2FcGqrc4nAHMLwhg1iePwPn1McUyZPSDSywIoK4zNe5DgChj8iwCuWhFUtbPzXtwIv9M25ejkojZhML6yjrJAeW8SW%2F1GVk7WeXK1miC3RlOyjxMe1&X-Amz-Signature=00ad87e20b3a3cfb33de55c72784d1adf281eaed4e7a326951d06e602d515381&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

