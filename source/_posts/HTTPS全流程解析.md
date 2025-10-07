---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VICT5K44%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQDkIZH%2FgPl%2BbCBdwzCZs972rZ%2FCIgP4AvvFyRnv1m%2FpCAIgInLM%2FEFoa9sihuqeKoyZoPIHbC5zkoutuxYPG07oztQqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMzJ6XqAmktIAHohRyrcA70Ai8M1vo07JbutNSzZwMnoEZzH3Ev8tY%2FmB9wfzFNz1Lo7WO1%2BVM9VQIzfleIllhhZ97DxkWde6ZgxJBd3EDHLomLLiSy3HWI%2FaPwbRTIIXQiJ1nj55YM%2BUJhJU4pOOi9%2BVWnMTGwGvWqMpsj7M%2BqLUqlSdRx4GTT7OmubHYJ4kSUevFSM48Ply%2B6NsxZtubqkDYTKmm4cQFQYde%2Fxtp6uBROiJrECTCKfbZ8nRnxGsmQMXSjtqgCBcay06mY7UO%2FSm%2BbWPWuWDOp25%2BkUH52o9ksq%2BSq7%2BYoZ2MzdA9qnA6GMUxp2YE6Atn3%2BnDzgHtu7FuyAvN5L1qgrwPdqy2wUev1H9oVF%2BMpW38fa8E5Py1bJdNKkaH6dkfmO%2Fz0cImwhtb3NZeOlt63LN0SNupopcQwlraWwvkYZhSFXzVNtF7RMalzHFxqTAPBuSfvAv0bnJxqMLxRA9pXRPswPRQHJIa5LVyW0SWFdOi3rG301nfOFMxC7nb7vcawcXiBZDb12ZWKZdhS9Gioah0%2Fkg04Ox5r2yxtTL%2BI6Y6CwWWrRnNAS06B1rRVe%2B34dSGFUz8kzCeBIUg%2BrSOf9rBATOJDtQU5bXvJZmquhTRjN73MqjGMpqdxzvKlnTtf3MOLClccGOqUB79c%2FLPUPBIkPCR7M2a%2BGhTYGTz5bVPSoZMfQyr6QqSRoRvzJCf4J53BfjylpG0xnVT8gglJMxg5%2BBlP2%2B%2BraUXKZE8HdSbz%2FWNtI%2Bs%2FcbSiZjHcStdQzb5QMlQicPuvo3IXieT1po2ODkRMcJiGuF%2Bc08u2JVWjvGlSlM1iq0Qrs1XiZ3S3v9B8qWTCNyMJtq5%2B1z4odHvUNogoKHc6TW%2B%2BbY5nV&X-Amz-Signature=7d8fd07a75ac1aafd646a310aa6754e35f5a56fe585cb3404338e7afd7f0460a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

