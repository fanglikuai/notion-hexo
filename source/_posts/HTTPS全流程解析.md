---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLCP4XX6%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T082105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYYZAgpLbF0KnydhJFzoqm7C%2FpzDZZgx3ImYbRk2xCuAiEA3rZcoPCAzbu%2BzTnDM28v%2FuyV6OmMswZ%2BmYfUxVXAIoEq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDCxJOJNB15rAcHKaMCrcAxJ5%2FS7cg%2Fu2U5iuHlTwW7U4zrRLFKhlsclI0Qu7bW1E83HM%2FvycNsNSxcXbixvf69NVHe77bucmnyjAIJUX2pv%2BMtr6I%2BR%2BlcG1T5Vt1xsLwZrxuMnuwG%2B8DLDPOQJlZMbV%2FrxWgyPlTMG7JO4eGO16JYusOjLQpjR0mOFJ5BXNIs0CZDaWIViQa7KHHq3tBCtWjr2Z%2B7lhR0IDRK0adK1aJrquz%2BnYT136eOtJbM0T9%2Fq8u4nhPldOjh03F2J%2Fd9NnjAw7A%2BrsAfl2Ne1BhK4BFqQ1hHgyHGMBcz%2BWm%2Fh9NUH9NR%2B648p3uAbsoJFTMU81uAWpl9s98lPf3Rvd3FQup6TevbpZsanBDzOk99WZKOysHvUW%2Frj3dMY%2BByunDi9AW2Pdlrl59ZWyD2%2F8O%2BV1xR8nQSVRr%2B1XSfZB1W%2FBv6tShvsLdZv5dF7ynzcLuTP%2FnkJ7eXK85xFcU7o%2Bf%2BFxAUO2uDtEuuKdK5lDhFoLWdlPOhof7tMKjHAdofWlmLfpYJbmGtqOfny26wLHbVVy4MFgncL36VmYlOf2JChG8DmS9j64%2FS%2BOhpAD%2FlOitQz55ZTx238hMWMjIlogK4MNx%2BNyghYAFHYj6ZHF7jlDZwDmB80yJjy7MQZwMJGTn8YGOqUBIopqxtIn0XQtcWIUImmRb3zqd0nQXqwdTgKTSv0v1IccHK8KN5ZKixgTqGD2N8%2FLK2%2BupJEdNS%2BriScko1YFQVTgHqs%2F0gsuNDpbGBkgFgLtW7giYM448kXv8sZNFJD%2F05XcMaa%2B%2FFRuzxvbtzftlCVTrDE3caOgdmin0nuUntaQNAXQ3Sd4DtmMl6ORWuE0NbStFraMnW1rb%2F5IucVcK72VadEU&X-Amz-Signature=26d6677ac7f9f2fceda01374e5524d8c942a25d9a76894517562cae7d2f62e2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

