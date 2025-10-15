---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CRWALS7%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGuYHNg6%2FXJ2grbUlp6oACoyQpbx5PaUKf0To7LP1PC6AiEAra76JY%2FJxsxVz2UWYQvpN8qi3MoFyR8tccg8%2BUtryI4q%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMY0wLCwy47GZ8sA9yrcA8wJ%2FRaynnua244pjbJ3ZZNeLkWpuCL8kkwUwJ5O06rWTRRsUcJ6fbsXgJOH0hvMusr%2FQ1QcS53HPffUqVKeQJGcrvRwq23vMAs1ylWYrE9G6c7ezzLcLNFLmthC5ghnPVefjbFUNP3HMcTOANeOltl5oZJZ7RDRFK01SHBvTVCC9OB6e0LwD3nqR%2BppEIstSn9wejv%2Fhwdgmw6qi%2BjeDpMFDafWpJpeD3jpyfLWtTP0Ikoh9m246hiMDVZm9MU9yGiZXwe%2BKHO%2B%2Fe9H33wgLI%2Fb13crM7x46l1yu29mJf1xTeeqVJkBedVexjKcAhhdLXDnqw%2FSwWUOEfrpyPxMUYhV4%2F7sIvKFC7FnCCGGiMNAJ2FnFzXQLSCMB7efx1RifL5kQhwdCU4sIcxbUHWqXxav3QHTWL0ZnurFrN73Sawk8f9RV4ZqtCZnS3iaF90w3FSTEBizbrEUcXufrv3cA%2BGp7QX3NR7iUwUjptiuyilhbTlY8Hgh9YujxUFJu85VArGG%2F1qu4FfFN5nRt0NDArhqeYassLUxKC5s4Ek7GWTyzi3Yd8EbA2xOqvQgTlE056U0qWQ09M3aB5nfxzrJVXbjXsIOVvSwsSjNiZqJXjxe1YkGMMrK4DzX5PEPMMH3vccGOqUBPGd86evhXxMHnlr3T4qKNFW0oI9AFna10MRMjG5VJyT79jlahT5hxwNH7kpA2UAKxsdHZwSKs0xq7spDJmwWAzIeHO%2BkxBrAf9QZUuG7lzDp7WEYDLFbnJqRrvxSiUIfw1G1qS6EzNqP5y0zrjKWOONa2zf%2BxNHpffp2ZS1skTSuW5mRonMbAzaMQIgUFz%2FgpsaQfGwMUCksQrLkSn9K1gN2ty%2BV&X-Amz-Signature=c45753e8b1b03478b647d7440f6ec1a37c8ea087a1cf429927e7ac439b4e22c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

