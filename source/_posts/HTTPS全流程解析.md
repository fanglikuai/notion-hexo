---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSATOVMT%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxCbxXONySD9sDGpAiD6c5wU9xn2gKZ3Ys5AMWSukFqAIhAI5CUGOKlWvFixH0CKX3MYv%2FboYMycBMg9IIDTmtA%2BCIKv8DCEwQABoMNjM3NDIzMTgzODA1IgwgkrCzhi%2F4bVV57cMq3AOCzHM%2FXCrfHtH3rGPyRcC0AchHkfOqcnq4P49H8R7XYFJIpMfEb%2Ferrp8CA6tz4GooKCc6dxMwYMuihR2N%2FAbl9HT9DRihy0%2BNwdvjDUrHrbPryR1w9IK16nhBPBnyt3%2BWCBZi2BNGptN5ctyfgixTAk6GVBzMkQZNg7snmbj4OiIrSetUzQRmqt3ssrL36mt%2Bt1dRLQ38EV65sW%2FqdXGd7wDuOoq0%2Baxg4lltEPou6AxHixi%2FAxr6qgILZEwGQ8bG%2FdMnv5G0QgXoHHbqBBc%2FKln%2FJonRv2LEZ9hPEFbqOgapXUtdDz31JDOf0Xjpm0jv9wbBpR2MMQDG%2FDEEizfj4nnpqxFcQGFith0732SNhl2m8tocwEhUBbdFu11TVbvDT8qBrxcVuXz1qRxnRQAJSrQO4qeS1RNZ0ze5C5x3rbPqE83iWOjXmB1Qq1%2BNc8XzOM%2FJIM%2Bm69Fj4RNY3XFkyNaRNsmdjSO6pI3p1hoyNoK3eciPOR7qA7CBVOHtSHSBJSUi6gOsezOx%2BYVakqlKNVH%2FzYD0adG6nrU2C6LZxlXvZwEqJU3hIIQokoN%2FSq0JCruRH9IFAiBFwMi7QeJ6A%2B%2BvnHiTgRgvP0IpwEJ8IZBl8zrxRZFyftxIjjCl3Z7IBjqkAVF5SPTotNK8sxJzMz235mpnS5YNv45Jzv9WN1dPf32AfYVJBeSU2kb%2Fq%2Ff8GY8KlHT9fYdRD7G2C47bCowtz%2BVbja0BartMrjBr%2F1G65vxY2gfYxqzp4XqDOup9p2PmGjbzXTOC8UxbtuW8bmPQDRISpRluixR74QSMsiwcg9w1lhtm9I%2FpDYNKKcBxpuR4yLMqnrblkDgWxh2T%2BIK1zAoMUqnh&X-Amz-Signature=2c6210669183890caa0d0ddbf8ff75e14ea1174ebc715bcbe9f1a7b46dcaf37f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

