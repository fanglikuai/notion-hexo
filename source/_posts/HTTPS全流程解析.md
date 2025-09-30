---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666POEOM7O%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIQDKNEkm9GK1qsxPH9%2BKxyQ8kDHq%2FML0sJAVpqsQpCXvrgIgegHkKp27j%2FI%2FF6VXa3W9bZowIkn4UJaQAqI96f3IVmgqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BUblcoi2EMio7P2ircA0pf%2FbCl6oQbfSHbMz9t8GsR9RaeEO3HBUgHwSw1HD70pXbf5VYJqkRk1TDzIA8KTQLPAvCalN%2FIrm7l1XhAzZS1nJHVECb4Zg1gmJ741f4yMm8siIhvKMZgjAWESEM3yMZTZuqHRhgias9IZ3yRdkYwAmysxAdaNsVhMPeD7XljVE3Vau4a2b6OzHA8KblJyjcBECUb%2BtbqZGK5nKZUDmzdXuVWMQu%2BShbEoFl3EhMb9mTj30PEXmMcMRcX7fKDtytlxKM1qONQz%2BTTp12oiDT60DX7ceNr6hRIFPUuv58iLFg9%2FoH%2BaHeqzOszWxAxKUOVJ2dof%2FZqc2XeCGVMouINMvZRq3LHdl4ENyD%2FMGI55WcpiUkTkMkraACuliHlBGXK%2B88jPd0J3AMmP8hHqReuSf%2B2pJRkrZZO4jp05HZjmC8KjhTEDM1ziQ22iMtBclU%2FB0E%2FJi7vz9LY1yM7yjC9czelmAieCG9l2qMbIX0PcmvK1xOAlI7omWh1oYy7Q9LJ0LaErh%2BxuUHCNVevMgRg9NcKkC2it10GwSnlFPtD119vsR5lcPEfO6xnAD5V%2BfE8RqPSfZSieS0bOVMLq2Y6HOX2N%2BcDjdJCv1t0r6FmmFx3L%2F9D7Xsy8kPZMOqp7MYGOqUB0OGizoTGC35VdOnnw66an2ZfaNV6INBQKrEZWLc%2FRnL7XQQSTE4HAIhrb%2FCwZKerKfo6Rxq1wA84nU%2BWvQ10vj2EIS5RvZWr4s%2BPknPOcSMU23VbcNjRuIanyDc56twv%2FJec0HycojQQt%2F3hJUXWw3Uhb3zQCOYSwhJF5RqZQuDoHYRXGnpUBANgq0k%2BuNaDXERC7DVj%2BgU5sXge12p7SCbqlP3P&X-Amz-Signature=d5ac469a2da73cbed2ea2da5581626be1ee7e313b90cd0f16f0b97f190083d9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

