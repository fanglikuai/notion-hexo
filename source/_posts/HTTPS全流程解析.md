---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EBLQOYT%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T140044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ6tL1QI8jf0rAFOCVahXeLpJbLIfEyzDLgUf4e8JmlgIgLfkadmb1f0ONqwakfwZyVR0nyf%2F617kIzPdOddOFXK4q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDGN%2FWRzb0ssqUwdJmircA7%2FpFtman7k2kTSPW7r2SMfn98Ol5yVp64WEtH3TY8zXnau%2FNxUTJcMsf5LfWdyNj%2FpooNq5NSK9Q7tgIWUifWkMNXynMP2g3r%2Ftb40yECBsCzpao4CXGxhJKZ6wer6gOMEgaaR0VnGcO5n8E%2BffRq9CNZPLH9jBwQLPLy0NXN1pYyMn4aPXRKQ6b%2FGXHr%2FyXbSPcHWpVMBsc4wcqhISOX%2BcdAfbPySVjb7eUYijnPXK23RHurzwzC%2B4d6Cm4dKmBfHhrZxGCRjMEFks7wLEHez%2B3zCYsCkccGOOTuNeUoiDiPzJ0Zyt9jSpxWovaRVEQ6h%2BiE%2BvRGei05IDjq%2F6aUdoCq6zBdTeLxbPn2FFZWoRSY3r%2Fsz5X%2Bw4xg32hCZa9fK8y8r3zV%2FF3zRTFSqBcZtwQ9jnJlDgsRE%2BTqLU%2BW45PUF%2B%2BqJsjNEQNeTMwtKXoiQv%2FEnWDZ7rjdo8e2Sff8v3H%2F%2BmbIiHa3panec9yawc9r%2BlOk3xb7U7eBIXI19RvhPQZtlug6R7dMSMD%2B2UdJ8jUrKSl66rMTTDsaEWVXFl1NAV1hLr9x3gmxOkt52Khx3AP3%2B2Wjiu2cQNUY1ks5b8S40djKUFHPWUd%2ByWb9tIcSCENRRq0%2BklNrqCMJeyysYGOqUBcKJ5ERHWlV1kcFkn28qJZ0nPOhj5dalJVxa5%2BNyG6X6vcQrf1N114E%2F9L0VF4hVLN2qluPPfVdHY7nFnGl3bY8OKkk8B8bBfG2FvgucWVQs4bYujFnz43C6msLScbFIdLM1aUPm2BSJOhI5sJnC9Q7hEr2%2F8gQNQ8wl1Yz2BJqs%2FvrWAwmh5OQN6VRe1r%2BSkrS65OlCKvfyRWK9nYVMRBupvXMRn&X-Amz-Signature=5e5fa0dea8f3b75b4b6198f849850ea30bc0ac7d4ef1c1f70a25521b1e00dcd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

