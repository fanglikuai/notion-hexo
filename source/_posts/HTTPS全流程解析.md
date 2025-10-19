---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666UTCQKQE%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIQClKUWkY6wx%2FmE6v7gy6%2B0zUBlRsH3Tnxl4dv6USKASaAIgQ9HyWyeLfSpf6MkLA5XxHcll4yFC7v6j8JnHXStTSuoqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBlFfC5dwzvvSCFF8CrcAzXFQ8F%2FMAlky0AUYOjUiuPPpL4shyyCtj875ux%2F6h6oFrJt91Z3ozc1hSNw7tj8y2%2B8Aev4Nm%2BMx2bqpF90CsdzCsq38yYeEVOPXL%2BxJY8zpzLqN1faWH2jULDlrK0ftoFVPmIUQ%2Br%2BMRHj6wBD7FTTpQfSdqsIdC9MgQNnL9Un%2BeiEQbUcJ27bzGCXr2y5%2FuUK5v%2Fx27tUD9pQTGtiN%2BU6ibgK8TsujqA237TQSBQ6jK%2B5ARLZAGgDfkR%2BzVVu1szhUHm5Fav2G3hvsggUlGsFBxNNcD%2BdNXOVHBPEbZjmcZnpzvHKGg32ti7IsVt4Z3YFk%2FsdyTf7zNw%2BUdLbWwBApoWpXqlsQ%2FDGY1EAq7qBD8SxeE7ON1UHVVFgM5LKj6%2Bds0aeAAeTs4L8jMwiXOkDG%2FMCuMIqqK%2FnstdK9GRkriXB79GUL5uMnXRZhlPmegNGIUtefz7kISreLExJYS%2BwUtN4BLKLVzHYbwjpNc%2Bw5Xwv%2BAkJAo%2FwZNeccA3c4KcjhBRdkuINiVr1zuYlXzARhKHv71ZBCQY2wlZ5%2BPeK9RNvlGD3hV2CSWA3DfgWTXq2JdwqS0VGkKULkjaetZjh4fzPc7kMTfR6uc8zQ%2BoCPrOs416IWS6WjLpxMNrt0McGOqUB1lovKZSWg6H9U5PqUY5HZV34pTA0ZwxCUUcHlfrFdWrIMyumZTO1dEHqfazrqZjU2YdjOiB%2BKT7STo7RV4dn19BTopOe5yN7YLDTEsBAizFBmNczsxqz7GqcB8D%2BvN6rXB%2FSPXAzgkqqH%2F454RyKUc7fb7%2BNyFRFK%2F7tufW0%2FooSzGQlLWjb5snV0TWq1ECUrvWJpopOw8wRUIAKNQUY%2BCrm0dKp&X-Amz-Signature=61ad94e7f894e948169f4cc986031da45604e3215eb136e605e837a22c65b3d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

