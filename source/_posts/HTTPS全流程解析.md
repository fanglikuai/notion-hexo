---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNBQD2C2%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGU8QH3v5BaJ%2F254Yzaq0IPpT6zq3vUjW7kNROTMQXWuAiEAn1TYROhDkTxbS8xChfmQKH5Qa9ji7BUe9D24L%2BLOdOUq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDDv%2FAJP7mOtZ5wm4ECrcA3Mzzk0wRP8j5A02GMI%2F6I0yzYwqHXmYnqS5yB1ajNyVTigPeOzTYs3GZB9eZyYeAiS3yow8dokPDXoNX%2FruVHNLskZ7SV5g0MxTPQ4umrnXUuot3IO9ZNqf9Zw8Uj3nUkTpFFjA3HCWdKCJSqVcvO07Eg1v3IiEaxQVR03kTukYO%2BWI5blAC9mglSZEF8hfDWk%2BK%2FjRCzyVaDAb5Kaxz3LbSSzLEACcU9wzK8fRVCUy66HuYOWwtbR9gQkz80fVALWdYbC3Pa%2BaMuDyL07mYcw39%2FZ3vJkuYNTerpM%2B3LzlTxBcJQ7a2BDOK7n6WMFhpFxVE4fKAtLsZ25WAkGe%2BKN2PA8%2FYfk3FkbPKPkIYemg%2Ft28Vwirk%2FJnBfNnuo%2B2FB3mrbTnJZJ6%2FQ9ETHi1wZXgf4WXkbafXNsPe8Nbyw342Km%2BHeQLFhqEbgz%2B0jRm0Yt0Qsqox5enVllMuZ5G3ZA%2FqVA2QJJXpUlyaRiFsntK2k%2BQT3Nrsu%2FPk2DSKBMrE9QyEjos4MQetbAaV8%2FWyKCEmb4IeKXiLK1VaNjYRNZiRv3yKbQq6DJDgVoh7YUwLz6I9OYeGCumVCQxX9fPyyl8GwLCynZtL7d28vk2JIlnOzDiEdBabMGia1lzMP2Q9ccGOqUBUEngZPa1bwoSCJKVbykWV0bKRIXdswYLhBwD0bNYXchObuhdwczWSaXuLg4pOcWaY%2Fnm68evZ%2BTIlEmlTGnKksX6jtFhAUuIiP%2BajqiKImd1I58zgD2NSLqq7xiIlrXUFC3tMI21pI3ux7ANePtw0ayJFzg8gBdCmYjDBa3G0TWIWUZyqtVjX%2FusMoyHUTpjVBMcowXhIvSyt290KNczqYZrWn9Y&X-Amz-Signature=64fd171477bbfdd243efed4a045418cdd0b5a4611cb986b8e65285ca625e33b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

