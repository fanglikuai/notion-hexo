---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7SDWVPG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBDWhTGP6Ta9BzPHM6os%2B68hIlicrUzdejbspVEklYFQIhAPFDPORkuv4X4tmb8Os3uaJREtPAzG270JJlyx18bt2vKv8DCGQQABoMNjM3NDIzMTgzODA1Igz3mPa2%2F63lflnBFgMq3ANcK7peP0YphybJZg66uV3fqI6FpuKoaZ9ZQo6dxuyS9oYa4mSe%2FgZz04bwU%2BOECztQUbjCf6OWroyBZBxO30EUYSQFoyM22mucD4sVxmzH7SRzsngI3kZZ0PbTOnfMbTYctuJa6vazP6ccnDvUVeElj%2F74XLTjiZ1QIiuWo7pXj1mEaOokY8b2yss0iSpwAST7QO%2B37Wp%2F%2BN6%2FEYUxPbbgjK8BENnEU3utr2wjGFDpE5kec50VtFPvC82PbAYh8b3XAm9Na2oV%2F0bw9tGq49uxI%2F8yrk15f%2B%2B7bgmlExwINIbUgnzgxJU7zYlTGAiObhLtFBQlfI8n2RDGAAIAmKbDM1Qfhz03kAqMSLouY0aUwBLeXQd8ZqmmF5cRp72zQFPVpUqMwraEfQqbhr97sexqZocVFS%2Fu3d6YLkY%2Fkq0zZLAiVeG6bKHeeekuxTylu4L8ovSIp%2BWosbFemi36g4hHEPUJUp8kT7crsvvPpha1NuDwErRh7IXjTwyfpH9SPU1KWLVRHEtsYs5XCF9jITZBpJHHBwkm4wjswGqbhDKuIUihpUEgnlAa7LQQApzhuaCsWdymlebOizIm7lqkyVNp3o%2BpEYEW5rGI%2Bw%2B5kKYPpCjaLgDildmuC4veVzCEke%2FHBjqkAUOLhLxeRn7t2GSbMvPd7%2FlZYyTTzsLdu73CUoLSDAUvnlvuIhXXp9nR4vFijow3YUJ%2F8iVHONopln44kqF4J3a1FYkfmZBgRR375%2FK2hOaYxy3uYbqcZsAMlrf%2FsrSppRO4ewY2KNhHep6OmqYhduNkU12tOeNto95bRL0ANM7Ppq%2FL0s8fTX4yfudA0KpdAdnyqOwfCpHw3GpdmSX1B6mRpT%2Bi&X-Amz-Signature=17cd4dcf4d9b893f93cffe3de5f1108890f6aea11d1fb06dfe8bf4210d20401b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

