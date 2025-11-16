---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWL3IZL%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbrtZNrmX%2BvfdTPdDRvRNP%2BvCblND1r5L7R%2FE7JoFlQAiEA25HiC2DPmx8PsIB0jbFNMHyijNglVjGbtYsvTyqMSl8qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLDIT5qgNbMnt3FbUSrcA0MtnnL%2FzHNz3bYgaZXRgFWBe5iGV%2FVzo19kMG0HXUWYmoaK8MAuIWNnZUzNHFi4CbfnuiaCmS14ytjYopuntFlY2O0%2BrauZMO4YWlLdYA9MmtQo%2BGQrepagDGQkfMQED4p%2FB54RJcEQqUPpvynD7425MznIM4Xsuacgj59y%2B8U6tkNOo3nmwI7fktoFsR3sRvoIlknTo7KFmEOPiMRExV8Mfe5GlJRoFqyUpM0zsqpGakObj2ARb%2ByeUo6nPMkTTkYHWkl5azi8O1a3WBlEuU0IeRk%2BaoD1cPbygMvRyNTxiB4SORB8n4P%2FnEerRwhwtCV%2B8bDOm4No78Zm6OXXegXDqevKcF1PI2DN3HsezdOUixg5qHY%2Fa8iuP6JhhjHENsax5AYD0vVG8f4OD2KlORtmpOA6C1xZg3q6iSj0Alytz%2F4CLObridyh%2BPY1apdIAsP0aGHwSSLpO1uZKTef%2F2C%2F6JugjhEhTIWwzlrlBDlV2q7e4hmJCpVsEaK%2FumkTo7L46JMENeC32nUa4AoQXFQHopzu3pJwqmirC7zkIJE9xeRNn1AdmwhzJHbdi8RPg7J6OlQtnfmDdBRqd94wGHSDo9bEMEFxZf3xUmR%2FNZ4B4G9Jd1YmQkZmqa1JMJWc58gGOqUB6tTTNgwGWUsnSDgoWZf3EfSAIj1T%2BEH8VNA9n2VvSJFo%2B2aUyptADRD6n6zvWwR7mQilsUb%2Fpu4d%2FyTm8xpAffkJAJ0SGZT3rJnhwozWugttxyfx%2BLhCAt3tRHOhSbp1b9X%2BYjbuInp6PnM4kn0R0ebPATqmYds1N2hEh2jwTeW%2FdqmFYGPR0cOW6%2B5roYg82WKqi0kNHZn0QjZMQsk467laI5WA&X-Amz-Signature=72a5bc66463a5595cac0b9bc196f503beff12ea180a0dbd0381c4bfb76abd7cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

