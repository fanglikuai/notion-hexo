---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356YCOEN%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T070041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRv3p9xS9vK8rhsc%2FHF0wIypaot5b2qNJ%2FEKIgCIxL2gIhALC4ZFu3RGEuCZGEc9M%2BznxrqjIaYwySrxOx%2BHtP3e5JKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRl1bUOjWydTJSAtMq3ANo4xlhZXO4iSlde84fHM9%2B5ZeUk1uESB3YukFcPYpJMgmLmpK2GCNTRSApK3zGp%2FD322va41MfrcrLY9vXWr9QvR3SnYQz1dZKHKe272ZAFd9F%2FDAfnBARhc3K08qQXygZ1AzlzUQ7L%2FjFMP564XILb9FsLooFpboWtbCrWcG5j4JpVweFNgi8nrvHvjMESNFk4GdHNZsnrZxNGug9fbvgrF4kX0%2BDGlsHTnc0Lecfe2aTr6lpNNJKxPcLfwb9ke5DgH8Mkiw6%2B%2B6KG92%2FGHsOwUKxvZsSGneql61j6nmxUEXzxdQO86v%2BUC3R7XlGcjlD%2BMs5h6%2BZickzD5trGyJxSHHwiDrrhrbGg%2FsnYLtpRzd5q36%2Fq1DkaNIyCvAV9MatHV6H6rbIFPpQHCvQcG7WTId5dQcJMj3kOYgYfgyuw70AtJdJax4WZbnPIVMdpWKQFBduSRVLkUzKknu3Fd5mCMAj8exd0LRZmoue%2BLCxD%2BsqyAHdnIGeqmHx1LIfTvqQnSXHADYDpBuAzORgJIZVVT6T02N7Zd8f1PDTe8cjLRb8loY1M0T04gfgrIG2vQb0%2Fdh5eeITH2uAg9IZxv4OLecVI8CjFl6FuhVnsz28%2B2JkjOoLRqt0t6KmsjDB2djGBjqkAT0zISnIJofSztL0W3qsH1PpUHMpRVqs8KC8cSTLJjxxpnN1oFUVqL2HDtWKcaZXZDOdA%2Fc8UJyPkPbvP0cviTP6yC1Stoftxeq1H%2B9JhPJdhJCB2v%2FdC5PlM8k5QpPDR0Kg%2FKHI7cV%2FD3x6HTh4lw5KP1TZorDpwgHNvyv0PQui5FH%2BT99KJwThVHgq817ZZW21gg%2BdB%2FNLb8Ho7Hl926tMjK65&X-Amz-Signature=8913c1fd2a6b2ded96abf658a284b51ddefd8a87d3c8217c1e759c5a12f5a058&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

