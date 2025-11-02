---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBJAR3GL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCYK8b0ddejPZ2YM9f4%2BdCzqL8sZUQy8tZGYzaewGECYgIgSvYojzrhtKvHW%2FMJHjY6LL53z%2FtwS1%2Be5N8nyxt0L2wq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOlcvxg0%2BfcTCS5uoSrcA0ra7OUjr7vTpGra85b%2FPgMYb6V4Zqxx7rFhAxnsR6LrOZb%2BqsNCghgY%2Bqfg9RSdsf4cDs%2FaYSNHNOV%2FPdSEHjBJ7rY68R6xdPbtE%2Bl2BoxiTCijQW9FAD6PWKowPSicVNT2itrbPA3pPDe1e2%2FpPjQVxAE1AXi5ZybB8cWOQc%2FWz%2BMSFRWcKt7zeIbqUWn1Ik4fzbvznZHKx9YTRH6vuxnYUZVsyD%2B6hqGDuG2eRdZbsn%2FX8Wrz%2BjEkuMio8C3m4%2FdQOpsJhFKpJy0T5jVdJ3%2BRMd4CbzvB0RtUSinxSoOc%2BsE6N%2F6jHe7kVEf%2BJHu5eawR2DSpKN4%2B%2BlzU56kp3VUwibk%2F1nTrXOWlMGTsOIXiCjVfxP3Aq2upNvySQNJMNJdiZEZbqwNQhkpY34rB%2Bfxsy1U8XwsY4OXMgWEYhNdsWnAXEpG0vkkNXbpyOZwNfNdzZXCTAusgCmdsW%2FTSwzDfp3Wa6pFK7UrL3kF1lZAN5cUAC137oEi5mDKEcLis8Y0QuhPzNgnp3dsZ9eHbc4Y3FbgqtQFna3jBH7agpU3vR6f%2BHFTsDkN16yXoW5HnWPb9BeGp31zp0V02JKQZYN4QsFbcyupTBCJrdV0mTI6l29QVP%2FrbPtxH%2B%2B9dMLnUm8gGOqUB%2BH19rBrsLC1anjaNKkTOwW78gap7rS9EPcQbZQjpv3k20l%2Bm%2FjdB5OldHKz2274KoktyUGfM4MMFSN%2FZQTX5hlYXTdKhYCfC24%2BAkljp%2BbLd0GkE4PjS6vDWYp8t2KMSo3F4n8f5F9PJDXbL%2F7%2F%2FbJSFE0B%2Bm%2FcY0lVaH80uP%2FjRGNJF%2BJDcyWtEyxGEOdGcPNf7429EtZbBnvQ9yYQ%2BcoBTKM60&X-Amz-Signature=ec35505bda5c1a7445291e592330eb5757ccce8ef5bc0dee548b32444167d644&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

