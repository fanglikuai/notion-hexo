---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UGIZUSA%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCICHnfpqI21EgD3PQfh6FkSVXEmqfA0rPlveOFeSDlTbZAiAmBCOeEw%2FB5sWJEhYDybh8rYTtJHMzCfu%2F3%2BOa7dOqHiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAREGrXuvHIQMmoA5KtwDov87Sa3N4H7TBFoTuh8o6UX%2B4Pfro2JhansCFaIuLVms%2F4gSmOg00YP2Fj%2BEE2vDI%2FGetwFTpM8jf8eMCgPB683%2BthMnwaLkWCsNnFyNfp%2FR5tYzpFCF11Hmu7NQsu0CXjYP%2BIIV9%2Fv3ZRrb5ZGKmeWCjjMfqbosTR8VtSNmdnNRfSVOtFLI7WLFpYhITti%2Bm78ipkVcXxfEoDLNHMur%2B3%2B7uMVVwv4OrZYBRLFcwGWsYxx3EJNK%2BBzhZQkn3LuA9ZZ5LGE1gnCCFTLNlUuoQvYNGEIuhBtCueKWX1dMCxkHeI3rwOwjNpHOzi4v5%2FMMmoNEWcfEtNAbFfqCoH9Q76YSdesND0aKuj2uHFpQxdbyMcWJcQ65iKYJtaa18KFxYuYv%2B53rLV45b0WOOe2iqpXj%2FhmwE91RaPKZYHrmbyqoVEIbY%2BkuV4B2znp4fstOgzwEjtea5o8qi8J%2FxQt%2B1VHg%2FqOXjFWwnJRgDuGvmRxSRjEU5umYNrhXFjvX2cczv4bw4ebHo83x4J63HF4AEMAn1%2BDmdzMOM4hJax4PqMCBGMAadNf59PiUVZrIWE%2BHL7EYQvwDDnNpxQUX4P9pn%2FeKf7gf2y9n5wbYwOM3eXKKaTw%2BACE7kjxOxXwwi8y6xgY6pgGm0LbHrltlEdfQv3TsDFh1NliCSAxLh%2Fdwf9EAVdnmGtOuILDA%2Fn10xCiBpf8CClPctHB314lqGclXz88yR0Aa%2BkW%2BMlks88HzWROgrv2tUn0Le6xWFC68XBq0UdUDdB2whr2aVUs260ptkxMTQMGfkcaPLhaynAewvOfM%2Bwg2KT4trHWjkPY7bD6j%2F6eyr2cAyauykNBfvFbkiKcR5Ssx2uRVcfwO&X-Amz-Signature=e55f5b60cce7994687e257596ea4c7e40f5f48ea85da67bde2b472f1c467a8bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

