---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IW4DX2U%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRHBoPktg04TKMOXxhmoxpE3vJhZAyv7BQcCb1%2FTnKswIgBxXBgxWio%2FEUbGgtcVC8JAEE3UxfL8CMFNM%2B0dytbZcq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDPnVOl09xV629S7OvCrcA%2F7Zbjj7T29nrIoMbrrxPR7LlH%2B6RrHwJ9fj5O08MT04HLfbbx%2BNokrwZYBD1iCWQpSFeeHHS7jb3sd7ijILPv3u9SJ3gLrg45Hk%2BN8xF7vIgp0kqWcH%2B39L1li6amOkR3azW3Dg2oWlL%2FRQ5koXamy0%2FKBYyIBdq%2FDy3ZXCcKFvpanBEuU6sfHaWrp1Nq6H0uqDDd%2BIHa24qiOIvvwrNzRbh%2Fiq4O%2Bb8Hh7WCoJSXzqNDHa4T9l2%2B8BC7BQEeIweWzG5yphywzbPZquYFg1shQ1l%2FJrHC3z7gXpnO5c5lAWpK73vAoF7xewU07cR2wWppDrdxtPuy3QEuYyI3pKUtD18Gf8FlDm2GTcl1a6IKV7O8mmcx6NSHQCc1aR%2B1XaZrXasqbZdZJlhOmAxTiUTSWhcysbZ%2FL7H3Hsf7T3Vy4l5vOOfT82fojZWe8Zl1cPB4d9qBoiAaLnjmkbCsh%2BkjiPAHmMIb3z8zaBQHVbwvZc9XWd0p9%2FYZpW6wnsZ3EEM9AOEVL3I69yj%2B7phoJnmLsywBGKmtqgNILg%2BLVAdu4krB1oZ2UaeuCZGWWKBYu1KWMv%2F4U9uv99s6XMuqgMrHBN%2FuCoe3OSzNhlXcNC9AoUJ89Ces90NJ7XCpNsMM2elckGOqUBTqhZdEFGPQpK8s0IBGwrG9EJElZUg9R2wTjCoylScNRtXro%2BjNzvXyOKRFWSkUuVT%2ByBpzECp58070eKjs8L8Cp9qWUKdUOCIkQJlqjCrRSTjsEIpQysAzTNyFAZn0wwWgUcAs85eTWgWEYS55RL2pAx8zkhY5wGmWfxbQj%2FgIXQHDpbLxWtDUNlpWJqwCl6WUftOD7hqii9LTcZgNa6zTTz4pq7&X-Amz-Signature=db102619813ed179051566a380bf1c2b7195c1f51eb00ebd7683ed92803a4286&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

