---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS5FHIFB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCrdgmePFiEmDuJgmAFp1fACvk2TylrH6T91DDZlTDEygIhAPH01yiFg5HyXTe93LBPHNX3%2FHkvd1dqN29M8QFwoX8rKv8DCHQQABoMNjM3NDIzMTgzODA1IgyOloRNByJI754rSeIq3ANc0d5szjtUHI0mfzFPxetL%2BR9dmldzwU%2FhvjbViNr7IJotZZHT6iYeVVy5HOMOA9u0PjuxdnJlmKrUtKmGinn5o7zsB2Neas%2F2G5DsQHkfjd0T6JdkA8FIIUOnxSQMHYs3o9GKJl1ebIHiCwZ2Rp%2BZB1w432WzA3RGnvRa6YXPZWo2AzYnzA%2FpyapGGbDkLI6mKQ7eQ8cPu%2BL2ZRlUWbL4jOZGnDrxeyzUnsZQowS3Kelz5Ixq2hbo%2BIlz%2BNTBurtKoY6uBl6uQDUtchiZS3W0eXuU%2FrrTzrXpaHU17SpRopuK2pTb%2BI2gR96261yBigWgufrMHwrMS6dWEJuv1bLMtlhDfrYllnciNkWINRoKY6IAodLWCfTV%2BDTtY7fcCIsWyuSXq7G6nM35fC4GlnnlazJUsB8HK6AdR4CT%2Bo1HtEJeji0%2BoDfuXb6ZrUDd0uyhM1GG5qw%2F9%2Bgq%2BApr27E6JOUJDrHeEMn44bSINLcRRGPMyDX3ZzW%2FToV%2BV%2FW4JOnKFTHCjrpLZU8LpEsfbPkiTYacxFavB8kl5WMeKH47%2BE5MEwyFv2KAFX5FdOKUlXX7OdGttQDOv5Wt6%2FTKAlbT7XvwQBGDZII9irDweM1tFUfKq6SRmPGCFG1BiTCpn4nHBjqkAUb3CuEcaS%2F2fqvYMH3zoBjzGmn4wpYdVU2C9Y2xycMJV2vO6dz3iDOuIyxq3aHHk7HwK2BROvtsqJXeEeWO7Vkv181l5lkuHZwh4JQoqMaqUehjsN7dVQXBWQChjJyqyen4l16GicKuOtcrz8uPf3UeMsRJJfZ9lQP4EPdyse6vunofzdFQBYGielobSaRJKwqVRR4ROjTYs%2BotDS1%2BnNMifETR&X-Amz-Signature=626d6302b73cbe864d4c7c25b0f4af0ddf9bb93c5bbb6538a1450a9f2f849214&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

