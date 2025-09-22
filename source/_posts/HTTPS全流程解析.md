---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LCFYTLE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAaW4B1q1%2FFzcSmoppxq4Pmcg0Xlp%2Fkd1VRGwcrixSNGAiEAuE89FeAZnyoJCc%2BJ0xSDGJL35PHxBym1u4fSeUWla8Aq%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDFCosLVTycVCrqeG2ircA7NOt0yyNu2X1gJtk7aCs6DdxOma9RxpE05YAJym5uHBL51rGp%2BbDz%2BXO94D8xOIikqHnrV76Tngt%2BMOp7z0sPTclA%2BL%2FF2O1XmkZ7MxHzuAXOEtxSVOHb3eYjcrIWFkeEHg%2Fd9Tm3V4uiOhM%2BInCETDrsG%2B%2FTpParqP7avNP5OSny5WZYmZaCIbMPE2V86O1Yf1XMB26eY22Qy7d0Ho%2Fba2%2FlKRsoPKf%2FgObqbX6HdHmvTHdO1lbfH6YznbQk3jnD8pr53ld7X7za8pZbnLOS%2FIIGpWHSqLAbZ0b%2BpYXWpYi6WCEVpbJ5WdJSFcWAQLlqxSR0tqEPDsCb3WyLllN3OJMdi1sbt2nxlluG5cRLa5ZPbOEs7Eug%2FZlINOZkI%2F3hpLm78Cy5f3pbxQuRx57%2F68eRoh4iwpT38zrQbzD%2BEO%2B7Q4Aw3wkZEeP%2BY2GzNWbHw%2FxH4zzg2bAYDrCejRXMnSqVgu1zXbBuR6Dy0a%2BhTBxdjOhI3qLccnX7WlV1G2unTrACURXs4eWEC9iQU2LIMfWx9Y5huv1ccUjoBu06wnX39AbnwIDwYHKL2O8X%2F7HtChD%2F60fEq0VbN26tJ6n5ffWD6x14g01%2BIAHZl7IObkcI2Yf2%2BE94iAKriDMKj%2FxsYGOqUBt699iEHh1KvEC7oSGI5iAcIF4J1NZdgih8kkaMyVAd6rBBxzs5Pb086fU6USbBa0uodkCJhGeF%2FHm3NJc65aqCTR%2BdJ0Tu2UbNXP5yyIriRggtrROqo%2F7lRFCktF4qFwWY0B%2FwCQMyer0nS0pSkRFdsdXuRCHgo205r5Xag%2FnEuMcybUV8HINFyMd7oPYI4vjUtCdLSWoS75Vj%2F6j8fZ9PzSeUs%2B&X-Amz-Signature=b6238e5db030c9f45e93ecf9c9a8aa7e5260418bbf5f7cb458f0de388bcca5ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

