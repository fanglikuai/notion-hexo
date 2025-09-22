---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OKUQCTN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaTj5avkVnCiEpuMu6r%2FQJ2AgICsO1LMdYp%2BcJEUEHRAIgB7SND8kY58V0Rs4uCZejsqM6kevLMdA%2F07zyXOPELn0q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDA2Z%2BTKPabZeAKLeeircA3Fa0ugxzP%2BD6iQ31rfgkSaN9CBwewslCFVgy1TsXEazu8%2B2aCgytWE082N7JgdzlZTUuLaTRmAqkfe8J%2BWbhy1pd3VfLjS%2FAfOayy1I3MVGZ9W617f%2BfmyLS5thFLyhCZjwOzZ3MK2ELW%2FOZCtWMiartc06eWKXwT6boYNA%2FIXUTLug%2FyrLRt3fcdGL80Xpnk615wV4fSlmWNFZ%2F397kld7Sa3yF7UdrdnyrQwGonC7as7MsP%2FXwz68soJRZQwFEYDarsGVJRjHr1ubJNz%2BMKIzuKdfFXCbEaOjL1PRGptv9E%2BIUUW7wn3b65Gtf1zTkWlOuq9w8jo4io3vGK12czjLDx%2F%2BtJxjKeLIuGTtf6m4go8hMGRP5lDl7YYbSfyqb6P%2FjIoPKLcUbHAwFYfWW1TCXSpDR8pCtIpdxnD5%2BrIzpYVGjv5gDMUMwSxtSIvA%2Bf6q0iz2%2F%2Bj6RQv5D1cZcju%2B7HD%2Fgf59Hwo6wLH1QTyF32dPhn2cA%2FgO54P%2FCvUTogs9MN7EuiFc6LWGoYmF0ha5viurOlPsK8Z5M4XxR8OyDKdrQv7oyDdPyn4Reo78XjfOOdWJG%2B%2FR%2FTeICHxtvWs45NAtX7ERb7%2FtZHAy4Jh3WRPArYdG6T3o5Cu9MPPhxcYGOqUBzzk5by3d6ddbm%2BFTnUtZMm%2BBQhMvE%2F8fD%2FpE5xeJt494BXTBembjgVnBkmDsLE9YFXkt1AKbtcKLIsKySeyu%2BNUhGJZOgEZcJR0rfhIGOG35OhUEqrMVjdeolpIX4FUR0DPNmWLFOStfum0AEXOlJ1H5AK8V9Buzr6Xn1WOEpIcWa%2FAYQM%2BSzV2QOmbVWgDJRq3GsuQqDlBPMgkaCOkGF4AxtoIZ&X-Amz-Signature=d25adede37b4196b70093848f9a40537e5f4bd9fe42d1a03ed0c65e4fddd5209&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

