---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664STOKOJ3%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCIA9%2FJ3kybBg5c9BaHCggq1BoZRPtegiOyS6PTxNq6jgIhAOzUSVsOAmDTzVT84rE5NMQEa2cNS6XQFUEe%2BgwdpVbgKv8DCEMQABoMNjM3NDIzMTgzODA1IgxMYypFhylZ5waU%2Fb4q3AMHQ%2B4%2F%2FKRJqUrqWrySy8wPugg9kyyx5C0L%2BCFM9UNgj3RPM2TrqqN29Ge8Qe9lCGvM9caMzoJC4jKiyEFKPr2Hpq2f7ENCilh1pzu%2FrpgznJ7YRz3P0ZfWuf2ho5zu74oD1YYEnfDZGFut2NtiEjRn5iZvdc0sVd2qMP8jjr37ODTqJq4TyY2Jt6mcEnk4gcJAl%2B9norKwOBiFRXv4dKzrkZvx9aMD4E3%2BZDwnKvm1LtxZCF%2Foy3HuU%2FstIjCblFVfkBT7xw6DCTjmKAsUJpzHr94MqqwPQqiOJqeJ8cX0fdJyj7OfqRRKGRlrPdiaQvwDg36tuVMrZ4sXSDSwbXUBWyxa%2FUFQLSbvYGhQVs3voIo6ei1wqwuWl9BNwgTA%2F6HXRTTgqeXlnvQO7yvnSudfYZeFsq5s6iFI5YKo5yjiYy%2Bmbpx4KNDgLxrTO4IYnLzS6ZO6%2FZl8gIP5eNlZkmPR5%2BmBLXPDq3afb2SxqBSVF2nc8PdW9PyCL%2Bd5yKP0txXtA6C%2BVVnTE5yEPOi1FtttbH0tKv1GmIT7gJSqv08w9pjvgDvSJdyR%2BJ8T7t0X%2F0Vk7eZD%2FhDTdNaKUyHOkM8Efd0MvFSjqnSR1KYPRjUatJPQsjYAWHj7BQkUkzCX9ufHBjqkAVBOi0mnG9TsMcxjKfdqELUfU6OGLq%2BZmdD1L3%2B9SwPgZw01BHWlm4M9Go7c9h3iZN%2FvJ1fpK794DPnofiIukxx%2FKlUhVzMddqhCRAr%2F%2FMF7dshaupL9X6zGi%2BOvaK8Hv9ma5%2FI5Gf6%2Bo%2FTXuQ8Baj5TI8Lq7t2k6y%2FyqoHlsh8T0cpkGIQOGO9LyZdtzIL02DkbEnWTV3yWqKzgriRh%2BaUYAIns&X-Amz-Signature=e3150b8bf301bc6c4a0bb7a4af66cacb429840a8d335752a7002f6850cf57d83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

