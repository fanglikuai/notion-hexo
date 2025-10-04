---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627HB6MT6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRCXQYcQGWBQoQr%2FiMvkgolBNozJocGhf1vHFCls01%2FQIhAOqJmhVSxjMPUWsmGRKBsg%2FJzN4GZ481gkwLWnvM03mRKv8DCGEQABoMNjM3NDIzMTgzODA1IgxQMCe3X%2F%2Bi2Fuit5gq3AN5ZXz0DovJY9O8gmQEhb%2Fk3RCcvBLyxS%2BkNtNNojmwVvk0kbSeG0sx%2FN3FKkh0fAQufFzD9csiC%2Fzm%2FpfR6wVVfpUkeBESqSXaD7c3Yhwhw5iFop6wPuZeSXy%2FMejJEoNQCmp%2F4EjMsuAaZ9xfLi755kV8%2FIoypGYFfn6Qn0oS3%2FPEvqFngXipco%2FrDICS5erd5%2F0mPw%2FHD787xhHEGcAydQ6mb5I9kj8QunREsMnQuBE2CVjvVYr8JylIWB9kbQ8Rd63xwYoUKnG9iQmAN4ExRFgwVbj3RVJ4vUXrGaF0IVSXoX%2Fyn4WTuEI5fQyi0Ml%2FwbVubb%2BRukbTzFG0DkYAPaq1ITN2M8TnCm%2F8qKyYRjqVDWNvIG3s9zXTzt3bn8JvqrYys8yyASg4vdAqQJNIouUE3ZmFOAbXquupOU5gRIJb2AgoYoOhHFKe%2F5RibTgQASF1RQFIz%2BVducDAkmA0Or6kgUNnWxhpu9xX3JvdDxbRUwj1YC%2FCIECUWtcHrOZTfTq2dJOidQCmSCeRx7L%2BHmeC03C5OI3c6QRrLJJaP9gRgu0K7rMUJ4den4jvKl69FPNed%2F9KBHoG7n%2BkTfUB1ZkM1bPtMVyPfDEImNz9let3aRvQ%2FVsv7d04GDCUj4XHBjqkAdCoxUreWMg5fNCF%2B5rsDERfNLnl1m04SklLwTHMhIttlFDC44FsqjAzxsYGAi1b43uIZOU8p5vLjLrbrO%2BkTZz%2Bb%2FXlmO5bX4n1MNd6vh0jgvwk2kTFdasDhtlfMLsJiRYg6jVkQBQ8%2F3%2FC%2B%2Fae48q27GnW7zm4cVzc4RlYwgbWzQhmS3mwxrEWGBm5ngeJWwcgS5%2F30ouIKt8gcLEwQsPY6g1B&X-Amz-Signature=2c234a6af41c1ccbc5aaf41fe77eaef3febae29ae38e7d8f62600cbee9d925f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

