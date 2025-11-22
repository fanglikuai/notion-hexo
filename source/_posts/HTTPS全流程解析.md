---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKMMSS3P%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCpNmgI13ZSb0c6ymxng%2F53cKWj9CgUuhvrIbFzvHtqXwIgY36DZLOEHD3ExO6CJ8HvOANbg4d6ySa%2FbIVcIrdRn1sq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA%2BL0Wdyb9WT%2BNUAIyrcA%2FtP%2FqcvgVVmnmaMLymjgD0yRcz2z98V3uEx2HSFXXi6obd6HoKR4fgV1nlC4yeYqilKEy6Ruj0JCGcReEUI8CAqSA7EmxIVppGLvESmZ5W9Gcm76sT5JAHFcQMuTYH301LBXAsTUPMxVem3ek5XXttm3psO8ghbHfJFiZ0woEYYYYS6ZaL51lS5YDEh4QtU8R7FDrG2hTx919w%2FICIQ%2FgX4qpPhova8UgoDtR2sk3EchUA%2BfXHmWr%2FOm%2FQrQZQTY7EsW5aPbcXARQz5XjPOooJNMpTkXEc9cZ76eIN%2FevKhV%2B7X1xpaV9QJoh51GSkirpfb9dpkUu3BuxMAxmT6g2%2FFuo1kvnbQioYiVz7kAOC845oGjZ8T%2BivlPEyj8%2FAECR6mlgFZ0kDKgbmCPvp0gn%2FnO7w0Vi6JpvoUYaXj6Ntge1tQWeffWA3JeflVrnXWDMVZkRxrc85O9Vayop4slpFZt%2Bh0D2LcAuXzSrW3rLlPw00BogBLcl4LUBGuovtY57BG7pqm5j9MKzSSSnIk57EyM12xQ%2BnBXZe5yET4PEMvkuD0z8%2BddN83%2FUTrYPFOQ%2FQ%2BjeEb2NpYyqRh%2F12xQlcNNFf1EJUdk6FsUEhFnPhQY%2FlUamb%2Bn9OXRVRWMOjkh8kGOqUBxd2i0VUZF4et0zKulYPPkQql7l3GcsJ%2BaClnObdhDMEekXT%2F4%2FG5i7PRE3mUibWtRShPPLgehsRR38OILykpr6vROZoUob8LlMKto4fWiR6y%2B0xNwGPCl%2F96C9S9ZDUsEmVkpst67ff02o8BhFW5%2FMZXiJOppK3fuU04griYGb3J3VvrY66J8ePL0TTdp9tJh3QCrFlNKBJ3csr6eNNGuArUmE%2F0&X-Amz-Signature=fc1b277705a862b22fad3612a2609da96ae5d21a25d8633de42ed8e5b38d16b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

