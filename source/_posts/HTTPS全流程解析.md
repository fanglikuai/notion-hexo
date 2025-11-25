---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B76NLCW%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIExxAxg9RaqEmoWt1OBpXywNUcbid9a%2FeHfbj0BU%2FvaLAiBQRCTNvSpmGUBzdsoYsBF8QuAKXnwA8JNYWplKlkjmHSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMX0dWJFVdCUQ%2FZA35KtwD0brZczQ3cn90yFlETucoPyguHJW%2BqeuDKABIJ4WXiqtrCltXbHvi5ireHv3LvivCLj%2FmKTtwulyujs514buz77snwU9WX8fd%2F9CqttwNtwJ2CuqlNO2ml4VFULl4YIvNHYkiLbobpTPMCRr3qEN3uy3W%2BvKjkcxVyWNGIQ%2B%2B%2FsEAn1ISatNNXjW9YO1pOJxb2H0xmxCni5dURtLv4ga7fnxUcXkEa6FOuteiQ8uTC8cEzbJObOu7CxaMjW%2BcRmqKacUBTOvFJpUWKxFSOn7%2FDMI27%2FykT%2Fz2RZd%2B3a57585hihvBvxqc1r0RyuqLEUcYJVMtg9e1YKV4yeumLLKnxLtL7XJnw2nnxWGb%2BCdj8Js2AkidamPmDh3%2FrmOIPVjduffGgPCDGK556V1gCNcKtbHdJZF7U3MtWgBRVZuXEzTLr4vkFpkuLMkobF%2BBA4y5P9fiQ7s1Y43vPuAPqPEJcDuINKL8FQmta%2FjaPUK83Fhh2rHqir5eBqlVqOyFvMk1AB4SIXlzs0J5%2B70Gs4FpgUkE7%2F7Lq3BTn1gnJ%2F5o8mL6m7zIpOVlKFoPVh%2FgtLYiKPzN26xmFQt1tjha%2FrxdzMmWE8Q8%2Fqswf9LBqOr7bl9w9BAW8%2FnVqQj7tfsw9daTyQY6pgFLDFreuYSSBupbEXjA%2B%2Fovlt%2FLvS%2FxvUiWCN01D5UGQK4bgNucpQLh8nQju802cmkQOfqDGKBEjCrmcAc%2FVlno6vYxlgcVWxS58KyaCpRgdZ29%2FBZYjF1u8EeiF8dOMdA3ZTs96FY08D4mqcfTZvQeYlZ3veVL6gA2Dyfen3CrSIrt%2FMN50CpR2w7xKbg2vbjEC8fHF1ggUF5z0%2Ft88b8uPM1ctqc%2F&X-Amz-Signature=fd632c1dd007ebf5bf0d690b1855ab7387101bc1061176c915536bc0d38f2e44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

