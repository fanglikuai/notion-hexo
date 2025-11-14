---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHLHPLZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIANR9QDL4dBUGvlk4utpFxAI3hzAIVALJRZbc186S6N7AiEA3%2BwSBVniUqjKPd9N2bCtQL3WYpR25wX9rId47dunMLUq%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDChhenBz%2BIaHXeUzBSrcA4tASmYOCPjwm%2BiFvuMPqUC6rCF7HaqFp6f1lLbq4j1yCk7GkkX0VOBN6lL2GOG3NRkoB1%2FF6qj6ZkES0F464PeuvfAHLCaHGFIARCXk6QWVdVTUqSgwpPiNEenB%2B5TOqLgnfKv%2FJylhfoFaWFQQr9EeRmdlNWf8cH1XatMZDhCZrE8Ilm9%2FweMij%2FIzQ9%2Fo%2BjJsIA9K%2B5eUndKs4FN7C7E7Iy1xGP%2B5FOuB9BE3%2BiYeFPNQPW%2FYJTpU%2FLkeZF8eHXelwOQHD%2BTzVzCz9PvdhcXBCn0Ek6xkkBsLfM4Zvidf1emlNbqEjHoSwcNIt%2B33PiaqCa%2Fq%2Ba73N9SSdi%2Fw51idMYwDQGZpmgmV1XWoycbMGAlFhWEBXXogJQ%2BVO%2F3o52WxSMG1%2Fx1T%2FdbGKhLVj9hyIjA2hirFQDQf9NB2NJZnpfcraUOnTxrvj85bFchNV6%2FvsUesTK33PKqG2eAtZHtdh8p%2BLBPOr6rugHntTwYr94Ed890538QsTvmJ7jpIx0Bw9ADbGhxKlWrF2CtscZGYxSyXBr2r0GL5nhdgh1iFt%2FMkCoxiM%2F24HZTHVmMHoM0gCqPrVoEw9S9bfDp82MeG6ZqIslaaDK6s7WUz96kr5fRK2o4Q%2Bw3s80P5MPC93MgGOqUBM6HWhsCERQaYzgKSAeT7wznKsFlXQi4yhwpVYpgba7LeMGXN%2FBx7ZsAOO6jDagc0Zw%2FUI0PwImOC%2BIoi6c5JGHT9hDinIFuZCi%2BhroKofXJzbi1iyoAKPjYRqvMEYou60bbPdthF31iSPj6epnRsEkkHsB1lXfFEFG5bP5Ufrl2oYIy1VrCzT2UcbcPirH4fUfQY52xAYkTFBspcBGnzgnXyxZDf&X-Amz-Signature=1e9b1d6ca12b8c0de85f9b305156f312eafcb939496949dcda4034c8e047cb42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

