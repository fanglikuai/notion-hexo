---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYQRJLH%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQD3QIhCuuXPidDDXI6OWPA%2BfaoQ9D18PruPJRukvESVoQIgOoXrxEZ8S6vQvRqWP1Jn1N6boGKP0m4SvbTYObadmbUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJpRo0L%2B5SCzESGqyrcAxDXSOdjvSc1s7yruYKS5Q11d0E6Ubi7Aj0qGabJ9D2EpvM0pWzwcVlTx18O1mD4Q1PE66imADVG52tsB%2B4JqLOl5xt1vihTYPll2CfZQIKY%2BQcz6L4ScNrCLfL2F%2Fbq0TdKRBdZQPoeJ0GCoztyGoK07oXo5e4B9OBfaBwPM93EaJEATeSKBMrd29Q7mY2UxDLr8Br%2FLq5nS1e%2FARCasdJ1J7tNPpJhgFkmYS6TeKAlsFj%2BeXI9XcjSDlEzRAjBua1kTaKmS%2BvzZjNgR0Y6%2B4zHjcidHVxfwDv81clkx5Wzzlk%2F2D7X%2FLP7kzVaXo%2B1pLxRE6tKvkL1WX30x%2B9Rl0HSzGWndiAYT%2F7%2BfKmpbrd4xUzyIIbx2Lnyi1znInZZEyIcQ6x8L7fsqLIu3q4jpsaRsiODnb63pFOmtF3Lzcs9wuECvIQ5095sY1UxpcOaZBx%2Fj1GD4s68NUaRkoyBxLsRHf5IrPSUv%2B4Q9ph%2F467ykNwE9gEUbZD7GCA9l3sTJufWvK34HKFstmEK0BYx4c5vYbGxf%2FcEjwFCdifY9GYeWgsukxbCPY7%2BZsAGqJs%2BvoLVlvUGYUW0xsrjZWkF3lWd%2FVhrDOJ%2FTPY8J7YQaqBO6R5pq1FJ3MdmtMAhMN%2BcjsgGOqUBld0%2BwNLw17Tc1BYFrnOV9BRrUkxuxK%2FQQDAVY2MAjqRRhOyRehO7rsGuuAPCMGdiwn8kxc46ZAqOt3z1w97mWRFX4P%2BmgSslrz6y0%2F97U%2BxOvrXIzyeEiZMuEYNvys760mFhJoiU6rL7RgFLDAYUlOWyuTE6bOxJs8WEE6w6cEzVtVlZ1AshBgNvobHjnvw%2FnYY7gQy1w793Fo4jzZWMN%2BJMLyP7&X-Amz-Signature=022441f8862bbb4118b71e274767d07f999ca1bac9aafebfa6b5fd38e9b77cac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

