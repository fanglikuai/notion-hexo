---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PZPHIXY%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T170119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcPCZ3oySTOP9ZTHAte%2BRsKhlszUi2pwUuv45v40R6SQIgR%2BNn2b%2FccTdR5GQUv9ELqPanwOYpvMvsbn8xjzBM4uIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDNXXTHvFTcsbiaxC3ircA612EMTD7JjiHgINAJH9SRnJPAKyxwABSSDhQ0oMr8t9C3NDM%2FzKJTgasj9ywpsiNEBefnFO9tkWdgwFp%2BqBJkBazaIGbo940iOeMueXNqwnQjL%2FHQ8B7nlQ0ch51D%2BMzSIuWWrnBnCXwAEWawner4DXjy52tdfDlDC5DqPxMyr9447wX5Z5QJb1%2Be%2BUraxmJTY%2Bw6rzHr1YAYhqe0HqjxxUyxeV%2FACQxCT%2B8cIuGLw8TvL47pKxeQt673y1%2BjcFlen5%2BJBLZl%2BRKKhGBGN1sI4K8gDvRr2y%2Fw1d5NlGvd43vjtHLmiTWcvGzRP2HSpl9GW5Yqx9p1nc5TJYPtwMIvNBzcmBOfC%2FREUirXlzM7T%2BJWyrgj%2Fh5xyxg3sBYcN03oFLfME8XPjPZBJGLPiI0VekG6I%2F4IJdsmyjLj9PBcA5OtoiAakB3ZIWtZnNqVCN2AIOIZ9a2j%2BvBrKEKBLXbkr03n7eW7KTD2A%2BEUxY3daR7W1vy%2BwDoYiuklQCbNQXzfyACZfUJYPSvrDmTJWoUbNwBxacc%2BGwq%2BLs%2BCRGtX54jebPx1vuppBZ%2FmeakVgBhMf5w%2F6MwcbHxk%2Bvt7UWnwBJ5ZLA2Ci22X1Lw8%2Bg7HhLA6%2BcUNr6v2JjhG%2FKMMr3uccGOqUBRs5K%2BEM%2F8mwhuRl5WhKmbDbz4VWO8KR4WKeuwnScsVTZMjDBAM6AHvxK4Aukv6oOdTvBYlm67Pw5hwa2nSL9OYz4BY64DCVmhItK11DBkyXvJtICxFj8%2FjuM%2FkQUg7HlFkPbg7NWlQu7HwCrjj7ar7lBfmW7%2BDicPwVGj2zs8o4aMujWFlQ3h0vEH4RBgi6ulGxmvy1Msf%2B%2FvNziq3pBsVxBkKnK&X-Amz-Signature=e1e70fd12f53577b3a97c16166ca6fe4a16d8b636f9c82edbc7b893723460f9e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

