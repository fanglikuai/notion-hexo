---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFFBJRWE%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCID4Wk3E3KqnS94Jj2Kuk%2BS46%2BTevDlxWb612nlvq1opOAiBD7EoNpz61vjhkU2d02LOYX8vDDQiTpLwwQpsDBY5DDiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbejrq1rOAmlSA%2Bh4KtwD6gPH2RDgD75VqD2P3OvPgOfJsA0%2F8wqzoEbM94XMxqnEjXTfdGGZc2hoouSeTGmqTySTaF3cPklfonP4RXdPMrtNjXCNXJ8A6PX0OS8jDey9rxJb95ryWcEN6zE9q4GcwW9tSFf%2BycfHm9Lsma70JoMmb0POSd%2FJX59F6cq9lyONZaTO880KMndD%2FbKI1mOZtgtFDTgXMbGBxNNW7eyKlXBD88GSrEtz%2FDz4MmtQZu6QcsiCsj0rmkF86Xz2RlU4DH05w6QekyqTgcRMhnQdH10Ei8KA5nwKo9TjjQfAyUwUbGWOCPAnyFqpHSqPp7Y38cLiPuu18lAtEJtmARSbBWu%2Bq6tWLs%2BgPhFGiunLLtm9xOVZqQt04FLk4LMuepFI9ODBqh8FvhfekGvA4d%2Fw0mh5H7uc%2FDqJlg4o0jPF7qlyEH1UYuFHaBd%2BkaRkHX2xcC2375keqU%2FYs2fCyTHX88R1KMi5Z9FI6rysKXXFp3RSJUrtkLKLV%2Bajk%2B5pb%2F%2Fja9F37Sp3IHV1%2FLLzA4%2BPfiVNvti1aN9tM75Y3d0wVWPuSvWknbZRHX%2BFjPlNOzlDKkZ88%2Fi6JdWXF0%2BXx%2BRs1w3g7vD%2FlIaELsJe3GxzvTZVC%2B2MrY4ltd8eq3YwlI6WxwY6pgFIdvLTk76Nx6NgA%2Bq1whzOMMcGIOmtB%2FzXQdRKJ%2Fvqlzw%2BTjn1WdMNopNxSNHshjSFjUinoIOiQketo8%2BupibipSgQipMvxIKKks9mDgd%2BsSuBOsDc7F4qqfLwyrY5P1Q5NM%2BCM5HgwibueF6XSwYuawql1ytFA5iwtsPFAOkSM2sCr0k6VSw%2Bqs3wTbbs4ii%2F2ucMHd762xw799hy7xZ4B02LWgnh&X-Amz-Signature=dcc180e4ea2dd501aaefe61731da9d919f52f0c73dd20ad527f7a6c063dd47e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

