---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAC7DFQZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCB7paNpxcNjCLWZB37GehxsZrzDX65O62KVvTjd6NZZAIhAOXciaZVtZHEXyJ%2BzhqSwsRjTNls%2BXi8Ha%2BoQDyzPmyBKv8DCEAQABoMNjM3NDIzMTgzODA1IgwugsB1nDXvoIJhtWIq3AN%2FT%2BdFpahsXNx%2FZQrRJ%2BZzjilF%2FS6qipj%2BJhwlGu%2FmOe9y1bW64aBSn7zsj0CCtXdxDl8qSyTV8sO9AP%2FjUvwLyN71xXoGB7uUbMN8R7dsShFo%2F929XVmbgTcnzOdYH1cOoDIRtr6LXvqiPapG4Jmv%2FtJrw4jSFGIdpfbiZNd%2BmhoRIo7%2FiZw%2FOXSJT08N72FfyTH9n5R1hVYk906ky%2BW3ELRTwZJEo9UCifV6mF2D6TD2vGO%2BZrJ4GXHiPaPFppnDvXg6fZc153cTRXd8ZK69rpdwLCvdKqXCF26gBNjyRQpNXCcQJ58e47FHISpZxxM%2FDSAjJs7hGYrW5JtAQ8KnMw51BEY8Z1viuofMdnHDmBSmp6GG%2BMCZ5LhzPVqcygR%2FxpR6yT7rp4mvJGq3jv%2FAncrx9YGM0qi9gc1cI1Id8T15lJ7gTYg2jng6kiK0FxKaGtUJuG%2BLFnfhe8u45udD0AgwE356nFxpMYeyQeVlWlnzGjLm2zxC%2Bvjj1oQKUiRp8sL60%2FxkOO3Gwz%2BmIMoqEHp66wB2j7qxcVpX%2BxOJ93kHOIGBR%2BOYBumQSb8n8CSazSIhbzt2Zmzcgh8g%2BhaU1fCQhQxhPex9eWYOUIjkfry9N1yNX2OMjD%2FdXjCWu4zJBjqkAevUo6YkTHn%2Bd2uj3iO9A%2Fu%2FTKeicqV9g750mZ1kbx9fji%2F0vhp0LUIZhm45SuvvYrOrWiO%2BZKhtSEy1qo67glcaW9p7RebvFmewrJqCafr2JiqlrTyo7PkuYTAqCo3uxnaAlYp4GlXDiVENdwgCZIEoSCBFt72QQb3HFqQSj1XwkroPlYsmoekq%2BuPFkTBbwE3kweqQyV8HCZBxZCi7wYyKS6PF&X-Amz-Signature=a590466ec49f5be64a1308219a1f09fade51e3317b5e4c1687aba65c70f2d8cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

