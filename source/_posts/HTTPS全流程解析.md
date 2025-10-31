---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UJBX6VS%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCis3Brk4IshlAQqwndtIvjKBWpWeHOnvIWehiqCyMJNgIhAKcTkZQnajxACRfkjx00ryi1%2F29VJiKDNKYnHv9gxancKv8DCB8QABoMNjM3NDIzMTgzODA1IgyTeGSy7f6mgnZbIMAq3AOhIYpgkHR78%2BaEBBGi%2FD2jjP2GBGIGLq3ssGUwhV6WvlA6n%2B6wFSreilwg6n9TGa6vTKK5Q05p17dIhzmBsXgy%2BlUy5ekyL7VO00dDsptfEMXjiQUsPdpiN4G5zDS6TvHOZ6Lu1i5od68P8VGpf9NJ4tkRVkr8LHtsTxRN%2F3%2BD1NxADY5N8AeWGZhw9PRDhts9%2B4TC8Pe4h6uDclo%2FTn%2BbcZ5weyQrw%2B9dKPTgGPSyRI0Fkp9N%2FGekB0atpdrOVNJoOvmRvejo5S03ePjRMh5YUzh%2B%2FE%2Bh9vbaQlL1WlMmWQMhm%2F3BKycyRw0miOHvgFxts9U8jU1j5wxqVocqdlKu%2FUAzopVTnG4aDiiS3Bjx%2Bo9wdcQsWsmQ1eRN4zIBAHIYsCoAMWZZd%2FKZ4PDK4G7kU3Cevzyi9jd4R0m67x91gDya%2FmsgjbcuvwDNJGz85zpKnQ21cy3PpiOe7QASz3bDXgt1eTLd1%2BB1hbglS8Le%2BjZWU3UiwqxxWmr2X3yZpWJcKCQjpjHdJsf%2FWcZdXx%2FPPoLnDLylnI7wWIXAy20NZ%2BV91h2jQgkOb3jnJNZNhoM8ds3bvpP9z4yqkagblNOSKmZxXPvEgcZgrKqEGmEqvR8Ci35bcGaF7R4uUTDK45TIBjqkAWI%2FfEv92w210RgkThP47KFrKxHfxXzQD0uAhGswPnHjnqoKwY1t2wWuaRcJlwucoZSSR%2FwCBn2w3jAkGr%2F%2F6ZYJqqDwpW0BRx10t%2FNZDKVc2wfJFULxcf8l6MrH3h3Kqu3xCeDXdDKbzZzx%2B5Bwabu0Je1ESrJx%2B5OAMjj7YArdn3W4YPH8XQ%2BRexwae1%2F7ZrgZJmRQGAZ0zPSGMxDMuw6AjY5F&X-Amz-Signature=1e651909d9bfc5984db723c341133fb148bfa1c5481dfb5f281804eed6fd045a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

