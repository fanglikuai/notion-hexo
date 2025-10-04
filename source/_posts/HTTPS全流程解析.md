---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665UL4OVX6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNRw6x8bAdVGOkSVak%2F8LGjKzlYvf2sHZGCtsuFXNwaAIhAIBQd1nGQuCW0KuGhLQq4Pr9nF1AwwyiOsshc5tOPh8PKv8DCGEQABoMNjM3NDIzMTgzODA1IgygGmeKWubAdmQxaqoq3ANQrsbnbwPjbdFIqLAc3EKVasVu4tRXgdpDJ0cLoKMDypFzd94osT1VZASE5rAzzDFKL3tYBwQVspmTQb5XJo7MJv62bKEkdHu4gtIbqnvEJPU42lijSi4l9kIGdBgCgv4DFADqEibQFgqveUa%2FnpooBypAscxZ0UEqZk0mpv6z1zF%2FKjsLS35YpttzyyeGFiOnsYSRQmj8VihVdr7I9%2BZGbLU15dxNp%2FR7kiYPQSjZ1XnX9LlLwr6agHSxIhAboMB5FvlCEl8Iw%2BuPVLOuoHh1KIn3M2W%2F4Kz%2Bsb%2BvEGpwyQc7C1X8LDwJaIcdQtGd5i4e38OK8zaYBWQbRozbr%2BfSjdzHACybV4YvxVa%2F%2FDo26l4xMSoFyKHu8ct7MLoZFx%2BJtbi8EdlAQEMuK2O3X8Ekms3NpnCJ1M%2BaQa4Hu7TLI5%2Fqw6VXc4hNa0glIQqpbqBpvH5GCObM40ofK2j5N%2F8H3g1qWfTAB%2Fjuk7R1qOvCVskF6EPDPhJDSaFq6midikimaiKh90BdFG9BGkZKDZ9UtSfhk3xKHRfC%2FDXT6DUxaP5MR0pp3NgqqXe0yM5SpU85OQ%2FTVeEjQBrWn2wxx7sjqQZ2ukFlcfQMg7MaLOTEU6rYRLInTnOoE3lqrzCTkIXHBjqkAavIK4FnUvLyYa4DftLGJJFf4LG%2BQUI%2Bm%2FtSYWCkpnA0ikgoBYKrYCvbL%2BiVtiv4dlpZgSiYUPUMq0U9Qvjuw6lZDg9d4nqtaea2EcmnN6iXvCjHD4CwMkf0%2Bo2Y7UmqbGigRjIFWlIfna43H4OT%2FR4%2B0vCpEfbbVphg4MygIHTHB0qbNV2JHRZodGeTb5HMpuDOqhN0nSsBhI0GFg342n5Zuifh&X-Amz-Signature=cce7cbdd9e14ac172c7b06fe3e017a8d23f96088d69a6ffca4510773f4fad30d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

