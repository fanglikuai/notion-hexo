---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T55NRDDI%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIQCuEqbPzlIyl%2F8N0eS5kH7A87owTKf7ZxyiLer2zaR9EgIgZLe5zpZ8iEV4hGPYON44VgkpRsP4QSaurwYrrEEt1ZkqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BBHV%2Fq7eBahkER2CrcAy9nLltRlb4uPmrEVR%2BOcEUk7UB0sJKXeQOWKR%2FFNe8BG5h0rxXfgtf7gUtjWq4Nv2HVxRKS3TKy%2FZYukdn2vzKcMmBAr5W5r0NUH2JdsddxopzRX3kkksoi2O0QQ17g8dqSwpfuTzwMy56AgRNr6QAeQBX5mlWpCOs29OVew9tQ1QWBaR%2BdegRbLmm0Eps1Ksy9yWfSRShdJfWpgGmdZeg5l%2FCt%2FFIHN%2B%2BPLH1zUi%2BPaBgcJqQ7FZE0sXd1wqzagXmA9o77dyOQGJJ%2BMegvnLm3A2JnJUfvAuJXUBuj%2FPAoNdsgjag1nm4aJaFy88iv7bdPEhIbCp1iAkr8A8f1HzBCX6%2FfYHH40jt2wD7hAmD9fv5KyP%2FEPR4Iqoa52xh%2B5HJ7Ag3%2BhEcA%2BFyAuJ%2BF7xBdhfSCOgV%2BfVhHHLiPypizRg3CXAajbGTGCvmZlYKmZje8ca%2BACo%2BNiRfQhpqn6YbDqdXYmqV2o41cvZhkroxU3uRpasAq46WoqWk2JsVCrm3cR3lSJZ0%2B0MgvdNt5Kja33cKbfJ6TelrkUS9t9RuX%2F%2BrcDOqxcYKkuZraqev3zBXW3VXLe%2FWrNdOQAoYgzZr7N1nMDS8GoicncdzoovUYoAWq5zdNi%2Bk%2BI6dMMMfIgsgGOqUBwTYL9Ay0kJP1J3n0%2F0aOXMtMftbjo9QmdFTbQNQB8QXDLHUo3sknDZIsUJNq4PjGAETgvCkolRroEIOFtFFLyCsTN9UjxygkDIlDWWJTaw0eKRaii1Wm5CHyE41QL7%2FfGdWP%2FtDztCgFJv4E52Oa2VkCeQk0popfQVZ2t5mWrY1h%2FqUvCIg%2F%2Bj0tRD%2BnVOoOwUeIx%2FvorkrGoH%2F3TFXL4Qpso77w&X-Amz-Signature=d09796ee8f7c5c25d0ea4fd245aaabfcab69539ce1c1374e810e95059a7b5830&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

