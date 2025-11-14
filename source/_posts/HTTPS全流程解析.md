---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYOLKFHL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T200131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD864pbWMPPHWYsJng%2BdXl4lLysMYfciPnlJzPG2KEBmQIhAJvPj%2FlCszgF8aXU%2FXM%2BmBUC8aGUxkRzt9%2Bao1sIXhagKv8DCG0QABoMNjM3NDIzMTgzODA1IgzY7ziP1ojNZQI%2FGaEq3AP5G2KCrzFs%2BP3HJvJY4sAgbQo7r0j6j8auqQ5FnzZOwQ0JBTT2%2Ft88BcdDBCb%2BjS2rhP6VVzDeiMNGoPHFs5q1gTZvyWJz5ZYnk9GiJ1HEEqH%2BYXrx45u%2BKiKGKL6ksgntcdx%2FjHP0usohM8QTa9S9ApvCX860IoYQA8i0IVLacZo7%2F53mJCM7eynsbvo86YqxW3SxebvCvlYyBIJJ1ye0PZtdpuK%2Fn%2BUPmGXSSg%2BG8Vpx%2Bqw%2Bq5t0EDXaMKAMYql29Lb9RuRptYQE%2F%2BLagr8QkcbrmZtFzEMjLezopSfQXxuWUU4pIW%2FWV86QzhWMucSRtR1%2FEf0TN7ZY54JmvHrcTO4ftU%2Ft3sZZt9znAz1yXptJrEsm5olSBtETNbBu20dD3T9l%2FW0OiEjXiLw%2FjF4XiAS99wLFmNXipHIpjb9Q8iljI0pPzhLMpVt1FE0lj4AmpllH8IEBa9CsnTPAuT9vdsHz9hwcypKXTKEOfzQtP1pR2nQzzTQujObXM2eBegKUTCNPwne2SbU0Y0zdtY%2FlGR%2FbKx6Zb1Pt9kUkH5qWQM8KQ9fQO40Zw6MzN0AOWMN5nxa6tS5c4hrPyVhj15tA2KfRy6a4YQm5INFBwYfjN5DIWK6Iql23vSXJejDOkd7IBjqkAevzTED%2B%2BRKDgv7Bkrl%2FZgfCOSqoEufUGqPDhy198U23OkQbwmyxJOysKFR6I36bw9pqyGFUX%2B9TM35Ml%2F04oueT6hsYyxg7i8pM0WYoNWYsIjqAHTGKx1WtY1sdHQMMdPH1Iiq9sh1xGXYVBeynMa9OW%2BG6rpm2FjJq8vjKMHkiTRdZquwW4xrjLwHJULwQmqHD3RaDDmtnYqKdlr%2BJ9Ob0IjxK&X-Amz-Signature=3b07d4d2d28254e498881224066d9458b1d178af5aa10ac519cca307fff49336&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

