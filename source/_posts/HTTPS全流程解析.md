---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ2M3ZLM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDA9Wz8TXS781U8Jiw6IF1XjbmG1CEhk1MPNGn0UGgMeAiEA6CEQGQf%2BnGIkoGbOR3Xxl00O0R2nT4dMi7N0WmP%2BG%2F4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNutSz7psuMh4bUy0CrcAzkqluRfOajj2koG1IPsgibcEcirRcKWVdovmFnRau1Hg9QOueJ6%2Fok61WOD2%2FxKEN%2FDyBeZMO9FF7tokJzCXCvOSoOhrPj%2Bp4yOYiauuQ%2BEHJg9nGNP9IVIGlGhdOBwN%2Br34kGjicC23vZ6YVKIqhIJYuS7giDNInSnAuaLGxzS8tVAhmD5FEaTsgwFI80%2B26BpIltKLFacisBSILjQV%2F5cxWT%2Bw5jVUb3%2FUkNjJQDvj3dSyw4tD1XUeTYWezqLbvUdcIvT4ROPGxDfAtpyIBJ1psyWi%2F93Eikl%2FlWVgiiSR6oEr2TXPTXVNuej%2BclpY5BpyilHM8QAn9U65xSu8r0aJm0cYJ3Bigv7KvF4BMusk1mSSJNM3%2FretonkIZ8zz2tQ1wa1ZJeV%2BMY95ARJ3igwJBQjEmpk9oj%2BS%2FtmgSPf8TqsvVobsdrFhYo%2BpsVWyBqgRD5HVN5nvgC2EyyaSiYs2FMPu5DzM4KYjPA08llx0Cj7dVfGmwiXIXdnpQEVRh39FB4jm24M7b4DrzA96dMzSgFDF3SbNchK18jp%2BQNDcSc5poXJ%2F5m5jRgXFlYtzpIDU1fcr2Ti8%2FpNVZ9fHrd%2BkZDBKE2D64l3sxl%2F4mc0m%2BkeXzdDY1MMfiWCMJeR%2BscGOqUBtgne%2FUjd%2F%2FB4PYVUdxzFEDmoB8k4AbPNgxsnXCX1xpPls3N36Mbq07MR1Zze1igsVYNkptF5honuyyIPBIArIk9ltR9oXJT1ty02Fvu8muq%2FsOgOSm4nwBjRB4tBjOK9bOQKoPvcm1Hmka036qPXcTj7Dy8z6VX3WqJQCaXECu7ZF5HwtdFG35mNTCpRuPX1tuVnF0116OLkiq2139q1yYhUHwhA&X-Amz-Signature=852d93569ccc71e9b92dc20a6ada74d3b06a3f0ba07a1379d73a9bee2a692755&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

