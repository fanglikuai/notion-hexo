---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMCMLX6C%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQDQS3anK24sX2RS%2FmDvfjYmxNkcupoI2tUozhR519iLAQIhAMHmE111hc62R1gxe8YsnxyGJMDFnRQd9Esd8hbjBsVqKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRE9R82%2BMHFvsJKaYq3ANt5D7VTloueVM%2FXA72aA0nnVjBCDtXBEvjrkjw881GKFAcYhiAeGlOZLD6Wboo3QNqOCqiIXgW3iFdAGB88Etc2a3MjWV%2FYheMhQMhGGVqA%2B7a1Va1sWNtqEDlSe8IHe7IRaqsnaWYt0ZZ%2FZWOXlPmvMimkdK7WdI2YV2LtZfgErHBCGdCkc8JwaIcdxcH2jkwXbR9%2BoP7WKgRUzEZM0Mpwu2lXu%2BIXGkdZ1RHy1A%2FsuSLyYEVMd73Tj67hOP%2BoTh%2BhmVGqr2jHbVjK%2BBae%2BWogcLUWF1JK8LJAKJVuYg4YuhOT53eGYUbwFFnfdsYmp%2B4p%2BLnGqR9knRFusggXEcnOzza0O3D7%2BMept9%2FEyWjg0XJJDGJWwewxg2%2BoEHiJuf5iWceLq%2BAYGAbW099D%2BLME9Ok2PqgpjBqAyHiq4%2BiTwpgJan1gPbJN5HrXvZtLm1mhMq3UWa3FUnNYlXEHpYWSY2iXDJvRRD8shI25Aulz%2FXa0zRa%2FNasAWp1gAaOuso18VHm3yAU8l52QHt0iGK6%2FT18latVpfrf%2FCsZUS%2FIVezLyIIG65mEDh0NH8Wm%2B3NqNQFkdbv7TZdq1nwNAuu0%2FwrUnSGscAeQVxLPpdC1kFiM%2Bon6DUH5MpUAoDCPsqHHBjqkAUWkl4J0KNc2BzXzSIyqDhgHdVOOKdvwNpGuWvz4f7xZaoYO0llkftwiS4kRSzMRZ3LngBNaRWm4knzi8BeOIgvNwOLNwU12R4ue5Wib7FMd%2FjPsz20SKQfJ32hae%2FIIGVZmMxOzrE0btYw%2B%2Frj4aJzB5GpKVmLZSo%2F4h24LXyGZzlgpzLqeoLQD%2FfHoc68x2tFXVHlx0M7fbcYWOKOhGc7Vo8Qi&X-Amz-Signature=b1af31a0575e7ac217391c3c71985bc10d760b795689393cf8671ffff36bdecc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

