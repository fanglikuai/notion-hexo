---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRVJ6P%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIFpFOkSjjBx8D1Y89ubEMHrDnRzsqHbUnExq15IvRoEEAiEAyb37Nk8UtgIRoo7cSn5LxZSTGUtL3Zq16EbiPmLuKKQqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAshf00XvrXAAIstqCrcA7Rfn3ctEZCPDf0InczUr2nE9TpkFy%2BaI5kQITKtX9E5GGwIVaw%2F7Nu048c7SETowgcKdbrtOCcAfBptetwpExJd7s9dortCWQ07jqd%2FdCH4iMJMC4810V3LDAMOoEbDPlWR9PAu7Lh%2FfFomhFBp%2B855DNTY09CXaV2DPk8ovbUr3r1e27oiLRtO3PamYPWy%2BkKrsUNw5PZXlAnN4hTBQQI7kBSPP1pxIxW4SnJby0IqY9zQaEiZhIkGvf%2Bb2%2BZKNulSPViGtUV%2Ft4Rzfrg3Blw47cIb9ifmsuM87RtvzgaLc0sOgMSygExn0lMQYSXnOo7gR3BEdfGWe51T2wdHvFsandTd%2FgNeaY799BB3OeCBp%2Bh2yR4h7kqmiWgImgV3G5zJB0geoxus61N%2FFQFGqxJ%2BRuy96IezPMlSeEBToXdnLfyL9tyXjZqvoeGawdMDwvh6x28Jd%2F7ioGvf%2FCXWpOqId7P2UOUDPAyNCXkC9ushlB2qETWY%2BRr5C1MQ2OxITnEPjQYJdh2WsmKL32VKY6erWCWJiY5BrFX6mk7RuSDi8dZ9JYjaQJ%2B8238d2WvPHBIgN0gA5FVPy2i2stB%2F9oDnQNp%2Bw84uscYYVBgouHZy7OG%2FQs7QgZR%2B6aMIMNG83cYGOqUBWPFK2H1zTtIosTLgGBvjr%2B3c27HKQ2%2F0av8eQB6mYQx%2FakLRf2bcMx%2F887BAqxeqVWSBjgym9OeWIjg8rKN3zaeqMmxapaE1YLTqbQU16I9alq6LIQxlHh7rMRGEVAipxYbhnNlcXhu1u%2Bey2e1k9hpAQhiPiIrqK9Sh41xqgZRvfSZRhHc%2BuWgFLd0uiK%2BwiU4IgxTYtq1heTy8LO%2FV7s9T96Qb&X-Amz-Signature=49f93a85e83738bde286d987ce44300170ce4764483bef1888ce6dfe812be5cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

