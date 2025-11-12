---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJ2IYCSN%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCVKxs0r8hC0ZCukrErO8fVwd0mzGF%2FN3ar%2FnwYveJ7tQIgQlGcwN4X8Fi2jic%2BWcQ2eNW1fxhnlsDPfQtIM7Rdl0gq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDERpWExtJ0bYH8XxlircAyQdjq%2FZ8OXsSIYhyjy0S9K%2BZ69dIoUW%2BRvQt1Slw27Sl7RhOL6GxPXYLiETjX64ixHnVoiBr5aw%2Be25G%2BtGmXST7m6D%2BRGlgct2mxrrFOkINtI5lWdyniOdLfqVi60Rbupkbi1LVguz5EzeY9hBzNFdFv%2B1xZLY4R%2Fv53cGw5vbWP%2FNYGEhbX26JQX33%2BgTOWrx9CCTi39DYgVM9%2BjDqcpDgIdsKQQDRqDTxA5aD6nJcnEFaVUphyQAN7Hmcl6B255s93q37ETItrEPTkuXy1GvdrVli4sKi4xSw3QobBLqDJ3m%2BOLEuUjcyoftzd1sQzaLCrBaqGq2kB1j5bV233ozmkfod8UH31F3wiss9OnodtKSf2im767qsJz1ruayJm%2FjeL9Z0YVJlUKa5dTN6QC2uFE5IsZPaQ5Jl5eo%2F98prNM4fSmhTeSe%2FaotZr%2FT6JmErm88sDMo8i7oLVB58gS2qB5taUrCJR4ca4JiGBf6HIfx6TU3sYE15jkcf7mrMf9F0G67wDO5ChPSGWAeNMrlys9Cgft2rsw7kv51Odwv34RkumWVMfYhXzt%2BH6V1CeDE7BRhGGyJHVFipZVQ3vgGIO8n2%2Fro9jmPeYVbgFGiDa7p%2BC6bX9fwQJXQMJ700MgGOqUBfEDh8t6kTBcwZlYID%2FhvVFYA8Tgz2Qc3dFNcBw%2F7BAddl%2B3ReQ49eydsLOJbUoKE1kGks3IV%2FCR1JkG4peK3%2FdLydH4lie%2B4bSMLc3HD6jSHVwWyI1uiwscO%2FFon40ejiOYfnoeHUQCzuXPqKl7vQNGJFp7bgWMIbNfmqH3jZCOIq7w1Heoc51i%2BzkjWwk7V%2BP8aColYo3h0%2BykzFEyUik69WuQy&X-Amz-Signature=2893cd96abe0e4cf23f58fefc630f574511001c00ae10948376a3ab5de00982b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

