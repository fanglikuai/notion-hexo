---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY4US3FP%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIE6NOnyVYmP5dTOe54z2P%2FRajl%2B3v4pvxM9V37a5mfZLAiEA6xOIlPHdBpp7Swl77NmQIhMn%2FkdyAMB92pYnvMPEfwoqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKIaag4lJF7IXY%2Ba4ircA4tc0Y7ABWY93oBaJqNbM28Vhh5E%2BE%2FQCaR6AXKxW9sBFa5hx%2FUQE6UENThN8fJnt7Z5fNV4NGE88%2FgPlx5aD3L%2B8dv5ccXfLui3QMUWGYZDQXdKCYJX0CzKdcGekJXufP4Xds3Ug4S4bZ0OAynx35Cd53HAVySl7mnKbar63p6dzsNt%2FdaD2dwZ8ZqNL3dxSgyuf%2FdrAxH0bgJgsMwD6wWYeSe2NrQlq0QQKwfMhWEhmAZtFb9ycJoN7UCvvNFMQciJl1EJgjidowE1ECE24UkJ2%2Fb1FerMU%2F5NlaxGvXKjUQasTt%2BLOKTYDAzUqY4GYbUEUtjaISmqD%2Fv0vVQyvMPiIKIRXvIRp1jzj55LuIw8roRs0qFSbMt%2BFBly64B49IPtaNHrVt%2BCvCGnMH%2FhgWZF7kPpNkVAIJtNXm%2F40W1c%2FVqp%2FL13pmna8v8H4o%2BGaALe5kwsPL%2BtcLP58nLFE%2F487zXEFchbXlChXBHsBkJkNYkaFKr9d%2B7hxz9xc6qTyjHfjyL8cWMVY2Duh8ss62Mq4vO%2FHeuNqbyKtTPJtWO%2BUFirD8feWLUdD7nUhU9Lnogr5%2BHZvmVZcD6ZBqJLQhf180wYB7CuLj%2FRpmSu7KwlUqS1R4BPjcOoxO7RMOXClccGOqUBwiWAIy%2FEO3I2%2BTKc9ePaWFqxCLevTiilFbps9yNjq1YX5FUZeMiOW7CajojDSxDqQ0ryaArcdgIDLIYV7mJIqlSifAnJwzzhzCCQi4jmeNChh1e3y%2F8fVWmtOACTawSGY%2Bvltsr3Xw6R2emHu9m2%2F9POhZOfTa0gQHJ8OO5vdIvV%2Fj422uCOKpztTJCUJ6lVRZeJevuc9a4%2B7%2FbB8wpfze20gbOQ&X-Amz-Signature=da53f86a0a8c442786ed55c7073404b6a6cb38e143553a9e21f288f3abee31f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

