---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHDWAQDP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIEpgg0KmTVrHUn49wCGDi4ckxgZ0Qpmd1XHvLrJDzrmKAiBLqKdj%2F3gLwyY6aFdd%2BQxqNqrx3kkwmU8bMrhwW4fp5yr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIMNtt%2FaSBDwn45Zks9KtwDqvjLGzYYuiMtrbGsQRYMJtpkbmNHrpttM7woOX%2FcugVrp5FzGKMwQdn9KFnSY%2FNXSfIUZr25BH9JkQ3IT%2BJqZ4JlUUapNBc%2FNvldR9WU6cyYXBv4vzOipKRv8YoWfDL8cbj45s7dbDu2CgA9PDUBRZQ70TI%2BFJk7RI9%2FxaA0GNoXwk%2Fb6eHUu2NQbNnJ5JjYBPAYjmfUZYraNUmgT6t28OpG%2BIysZFzVObOz5qtY1xUVDmZKoSsVfVvLJ74G5Cha1L%2BCbjHf%2BdLtUL2xcr5zor27W1r%2BoZEZV%2BLS%2B5yE%2F2BNiquVNyMYBRr%2FfSCqfgp%2BQ5Gscfp7g3KeBf0Jif4UKlAhgB79u9cQngeymnf76JWc0t7XE0jSKEGI5%2FCUiaRAukYwlQYgln59PCHZalMGqZTDu47Gqo4FPQBvx4%2F3eHs4jH4HrFPahKhcTq8JyIM4zSl%2FHjUGlVFL0MEOjk6NEwsz0UdfbCn0eR4i201C7opcVor3yoa02P78E%2BjkHACjqM4SJcJGvMcVfZYljZQchgpxgItQBnWvnqlMZU3VE9gP%2BpKlUiKQYSB%2FFAG%2BLoqSKvgIi%2BN%2BS8bL7Jiw15NtiNyFEkXX2AIKMGUaoIJa4DWn3oKRoafUdiNNxm4w69nUyAY6pgEL%2B9R5m3XImowJIydFRoT%2BmRSrNX0XhhKY6k5n5bFSRWoBjHI%2BtUiwdvNNm4gFEOs%2BgUAJp3JR4GfA3zXi67OOk2%2Fm5UGAjreHyZJbE2wSHLu75CKyGQfyURXBPnJ00WTt7TbdpkE0mgAnqMsVeLgOxtyiIok4r%2FvungAIQa8GP%2FTFpKZadfl9xtxuInJ3L7XSEUReXpabcTNWrDRrQ8lEmzPkKF16&X-Amz-Signature=2fe598f671dbd13c5e7307655fbe9617ab6c8f268dd24408460cdf3d1fe9483b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

