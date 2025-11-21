---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRJUL3A7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIDx0q7OyBUXLk49VtYPix5m4JvFlXeUGzIpnhoObIlkyAiEA2i%2B4TWPqi%2FD4KZuLY3zzJSEHikZ7RVcRx4wO%2FdawbWsq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDLCLJ0Evh%2Bzu%2FCLLhSrcA%2FQ%2FBvlDxsfBQlD92hjgthKXF%2F%2Bun3W%2FltiAclFrFqxPL%2BjFGskyjUiuJmJWEttERjz860xMc0aDJzNA8Cmt52sH%2BwYzOzfXUMebyv5zC3HBs3EBoIUJmXINbGDJsN%2BlVOrSrg9ExM7DaHhOk7Rf1pCMYhYW68ALh4ds0sdsyqoIAJnsfLeo%2FULtlo5r1qxTyn%2FNXi5Na8YUeqkefFt6wolRBEnWOMFt9q1ZWst9mGSWpJf%2FtJyQlL7YT9fb6c8QAhptzWaeVZXE3Bytr0DmDsl7d10LXqVDTHLVSXL4A5JigZnyQPZU24QfHvLkm9O7n0kl5giGfFhpwMSfSDCTN3rv5Em%2BLmdcw3tog18T6etgXa8rJuGmWphSDj2ZZo0SX9HxfmXA7DZYrhFNRZ0ptrvYIny1BeG8PHOLjyTetb6x1voakqpmutQhim9%2FB%2FeP45sqcYHAxiAmv5PXl8G%2FcR5mmb5ih0F0gCjfLizoPwZG5jxht40vybl9Yiau3NuuMqROlgD%2BhOBAXMSVATLViHLfHlk89hiLCb8lbMXBYzN4VjI%2Bk%2FfpycRhRXqzZkvQT%2FBCCOw7GyEvvqTRZmOv8G6NaZC1MSvYvmarbqqhW84qk3dIpC0uE%2FOG7W9zMKTU%2F8gGOqUBQ25nirfj2L%2F6R7WcYaEzKsyFYK67193P%2F%2FWCax2vAPMF1Xq%2B624vZWZDb5V3Sq0ZRHeKYbiqMQp0k1gDS6W4W55w9T2UQ9rzg6kMHxyLO2d9jAVXWfWZ6Xx6nyUKDltVPlbIienzK9o44EwU%2BYfF7mXwUZ%2Fb5ztfsfrtN1vlgSqesHhqqjA7VQQPKaPz9M%2F3gIpAdbrGds0QlNuJTRfSsP0SPigm&X-Amz-Signature=85b44a3aed0dafdd98ac4e19a88028ad38aecea1b036ea84543572acac24a293&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

