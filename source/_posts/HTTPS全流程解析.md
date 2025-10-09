---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDO4BY3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T000057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQCTLGaSLpfjJoeTlZKzMoN3Zk0wNn3siIiBo9DT0oT8sQIgebc5OcTAx0A9wZuUSVULWih%2Bjl3v8slq369q8DnU2n8qiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2F0%2Fmgg%2Fh%2F9Y5G3FircA61O60EOOIE72%2BdNH52eN%2F3WKuwdBBvTk%2FPe3hXVAf2Cplzb7y18112hfgruQ%2BrPAC0ahEPC6R%2F%2F5KVcXMIHeu%2FSpqWM7tfqOuN%2FW2NmACLKbO%2BX3LphvJsvh43JK073NN9zEFUr1tusosbzk0LJZB0Ig%2FAMYBrUpG2p2QzvALfv2h%2F3ZO7lL1CNVbbTNJpVQr3eyL2K2zhZH2qGjBYywzuSBuxPkG%2BgWO%2FMsgFhQagCEJl1RlZi%2BmLxTFtQZfGhw9AMbovlOIINcZ%2FxkrwJ0%2BxVrzxNMHW%2Bsh8bvlsnNOgG9DXHu1ZGnNnIeUN%2Bvf5I9UpHZ%2BMPQDE8%2F5rRzU%2Br1Ggc4c3Z%2BinnR3p5ovqoo%2FmTFqKXUb9Qtmeenq5BQKqQgQPfHLpsSIcNYwA4OKvugub%2BYfXSCZ3iXi30kuzsmrirSWXxkvCKFv6rHtd4NuaNpOKWPKoajKWqEPfwkEJIPcyCPqbR5ge6uqQkpEdv0I%2BfrdRJWI55Hzm3Xzuu%2FNm3jzNJGRVs%2FUXYQGlyYfz2n8YopWlbYik4liXytbmkmw7glt7VwztweZhkOeax7vhG8uwmAwyXScfOHtouBKkKOO7QQ%2FMHN%2BJ1QE6w8Ca6z7bnWJdylDweebyvWGEdMKLlm8cGOqUBhsJRQ9it71q%2B5eFYNe6s63sppyVWC8wLksCEsmvDiThs%2F6cbq%2B1IblmOYWLi3AeUaIhww7DqfL%2FJ%2FNcZiAiRCdwlVMNtaDwtcUGMgnLjdTNqGGYZyCts%2B%2B0Uw5ti18Gm6tMVUpQ55FjTJUTF%2FX5LRz2BQKcHYitAdmeMuIHrHLXCQqQziVd%2BIjU1bf3ddL9SCA4vp1brUiugz6N1AKdUl3kVxJk6&X-Amz-Signature=1767fa4a5c8f521433eb62d561bc8cd8aff108db0e5953a254826d5aa0c1f218&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

