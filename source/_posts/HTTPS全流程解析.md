---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJOM65SU%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDr4rNWUln5%2BjhrUN8duKhAejUfQeE76XVTjeS38NqraAiEAk9bzAwMzunHS30CApIuzWR8tF3zBYwqvPFI3J0To03wq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDCL0GYSKOiIaCAWNXCrcA%2BhMqXY5hCF%2BrwBtk%2F%2Fga%2BJ8dshfGLeb25S8TFilhr72bJO5AhDgubUU8tvjAsUvRPnqpuF9miVlAnVGA6tA6BASS7qTSQTNENR8RnDYsuRz1GSgCp6KvtbwbWFmaJ3%2FsSECp5UHlVCmjmXKlQ6NkoxRgKi37InC5eF4PTXlDI1AD90RESem5jiD6i3dbzrSr8mHNR%2Bk6QC7AgPhYn9yxFLhtB7EdHHcOGoAUcILYM8EnFQIQV1Wi75w%2FBhWCMO4yTmwzq6Q8XGBN5SQKRPsTlzw1cdN%2BLcZ5pY642rIERQ7elsn%2BdHYwTo7Oz0THps1E0zGGQ5EURfqysOniNtEg2JsApoZyCr7hw5xE%2By1W8maYB%2FnXfuVTfQqvwcA8epBto73I0n1F6r0O%2BfpyKeNHvZ1wD0dq3MgK8BHmYHAX4ZrQyRcu%2F4dDONUoB%2FYCAZDRDrkRQQmjYmpKYSdx7igQ0WVzndKzojZBjPVfCtV2VkHsbQnupe8T3uj4e%2B2JtV8VIrbwHQ2oPg1OrP8QJ8LYPI5ybCYVXlfwHsqOBEkS%2B%2BsCKG6BprPLVNi1%2BT3yVbF5xxns9GcEqLdDZB%2BxdvfHwBMTCrL4ZRTz1Ct9OlcKOr3SMOCNjURv%2B8O0r%2FwMJvXl8kGOqUBsWbwgTnBIr93GcGHmFTfm%2Bnmx4Oh06Hzif0IHmg2hmL2tQxpmERG5FmwnnD5po9wvhgOQ9PmXEitcYqgzRG5y%2BXNmQ75HDKKK4%2F%2BATJCUR7KWxudUvkFRXgyGho%2Fig6P%2FZH26mCCIMdTtVQS8RLtP5xYujGmdF1CbmXnKL3AJVeX3Rv3M3h%2F%2BJmsixKHyqtOfxGAIkHXywiOVfCTpBMj2oGgn985&X-Amz-Signature=23ed95bd4281db16130c0e72eaccf7f7b1afac0e5d63e1882ff957057be7ec2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

