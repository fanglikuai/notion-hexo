---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EGEWTGW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrMp5pIUOG7NhUJVuqSSw%2BtfxzmkVp%2By1sTorJesFzmAIgG3shvmMhFGNp81r%2Fy3DYtQBkwhCECwG1FI3F7acQAxMqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCae8VJn681akvf6dyrcAwweHxfCS9Ngs9ECgzxGmqFVbklvQSg2LB4KsB7cHlcWebARVh4poe7hUuxqVqbiPhAm2Nz%2FGKfUArCMa5ksntxLGtd8NbOJ0bw%2BWQ3pxaqjj2KSWPoK6gdhFjhZJuGUgLKO3yaFDPbflDuGHQ8NoYrMzgT8TzFW04C6w%2By5ktNRRcJg2s9azVtZjID%2FmD%2F6%2FzeRqtcf1675XQo%2FqCCx20yF9NaFKRVXULjxdvRRzqJGZ7gzIT98ygk%2F8ymtV0IV0yFfvMoiUp%2BOp4endbnl81Fsy8pnlCAN77%2Bjh7u5NDjtHiOyNDcMqvdIgjUfDGMG83sx2Bxw7N8NSDM174LOEgSYj4Xe22z4Rr%2B7qWCPs2V%2Ff9NX840uCV8%2Ffmn2ieIYfWN3LKLPO%2FUM9tWEwieTXlCH2zpU0OvGss9fhdOAmaTUzhyeOgoyKGoxXQcJCQqkC6j9%2Be87yabc%2BI8LOVjA2uTpUX%2BS%2B1KSLxqWIWuq4ymwM%2Fg%2FJEuC1vWzdd%2BzvW8qi1NKzPQwzZuW7MKx7p2DLU0gtY3VKHHSR7VFkeCsW0EjtCbrdgoZtpcFErpKLJdHCtvWzVsW6B8ZGtxuCsGhdUdwmJ6jpgtkjw743LtIt1drD%2FScGwrqXD3r0pmQMIDZwscGOqUBjjVzNzhFzW5LNeeITFyoPXQXh4zfGYn%2B%2FeIR%2Bm9QspIQSQBXLsoBxdToTYV2MhEB9O8XpBP7Sgue6PE1RTvPhS6XmgLJb5K8KfW1ectQ7EYqNo0TyWidVT8pT76gSBOivlxjYZsFS8nT7mTXQQ3zqo%2Fji%2FsNeH21EgkCfXcXZKMY%2F2xxCk2l8WnzIHDls8r0qsXl7x0aB3fUlwPp9HLF%2FGi%2BdwhU&X-Amz-Signature=cf2f96768b715addd6eeb49b96e79befd82483cadaa8485a5efb3d98a289c723&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

