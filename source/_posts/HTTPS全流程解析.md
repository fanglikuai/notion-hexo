---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPMRYXDL%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCsbDj1Si5uiM9XDe0klFrH3c5q6XLLgh0%2BIhNeYnNRPAIgLNLnzc0sL4tUASPYauI3OM6%2FpwC9PfQq1mRFUUa%2F3n4qiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3VGnHB4px8jRe7ryrcAxWCK58Q13rOl5f0j4V1pELGJuZgDfA0SaBxEhjWMsaZZ1fpW%2F8j7ClOpDoFqeG3FhCrxx0A%2FkzY%2FhOifyuqFRH2MxSVl32jVm00L1M%2FJoi52zXrGyB%2BIMJU3kcPepCmrlraAa13vIGHTyl%2FPS%2BJ%2FCSwXwwkADEX6ZcIIl%2BGnusrjrTr%2B%2FlowR0w3X5DWonY5HSEcfN6oc4q8AA7uIdcXjyRtG8%2FkHIffERIdD3ZnLnFvJkumiruW7%2F78RPV23A69b%2BUrbHQIUiCJuLK35aVXoSCa2OD9miO%2B31S5ww65NTC%2BQgd8u%2FseAKa2wiCRku%2BybQntR207vJu4MnJKxlUuFP6uOYW%2FyuHMVMmKEJpoogvGleXxWD6zlmRsLv8uWJv7e3xuualaPArStm1j1rQp6TLqbzltX7Ac0kgtMY%2FqbbiZUrJgTKrXy%2FHyUY7fWQctQ329rrWGyBcWGWaMg5JMM3tcrUigI7eobAi7UARL7NBT8bSIFMQxA8QMxP4fiA2QUr2vQkCo7JL0z1uHQr1r%2BmzikWgjLmxyr3QqstUJ%2FoaDXzhuDtpqXimQlTdCYaAeVvP0i8K%2FCqsRTUoJ5hq0CdaujWztZh5MJ0Hn9%2Bq7ZxBXfkWfoPN99AVS2rnMPLR%2B8gGOqUBBLnHVNC7xVOJn66URvZujQJORI5R8Havmf%2BCxHDG4C%2FfUtRQ3NxkwEDpm0Fj%2BVj57SNLYvBozN3ll9b4LdFW3hnGVDocXb3EUogQuoS7RsA9L8HBWug%2BOUxDhhY8zJFjVsOhbD0wy59B1hdHgRD3dStVG2hdoS9BrTMxlOPqaCM70KUhn%2F0yk5oxR94G1wF8XkP6d8%2BUwTrLBJ1bRc1TFIt2qyrq&X-Amz-Signature=2455aa072d8ed0c2a1053f3c18188254aab36fe514b12a10c472b96bfd1ac9c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

