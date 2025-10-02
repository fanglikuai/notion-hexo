---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV23IORB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTicTPt24XeEVkVoSl2YAg0vNMJBMiwv20duJwc3TZCQIhAMqJUwO%2BvhuKyQoVzxrHqk8U1fO7a%2Fa%2BTYQOD9rRcyaBKv8DCCIQABoMNjM3NDIzMTgzODA1IgznKrEk6oQFMLXTojgq3AO2QkAAaSB1PR20wWCnlmK33ic3nMW2toI%2FVvWdLn76hzaJDfenj0eDkUq626c%2BYkzNSDX3vcEUCvYdfR%2FviAjLmK9UDQS3VCtuQ6td8ua0cIl083M7CPuQsCPPkSpnmZxs%2BqL7l3UyNGFnWA2KeueNawnTUfgKUzXYabCWdZp1llX3jOUHykTEsteiJ59SPPSdfdwu7ynvzgfy0keDHNaTuDaLB1MknNknd4eFftiSApH0O3pOK7RpYxyG4WF%2BrE%2Fz2ECu6zoxWP3V0G4ikKKBduhxNj%2FeuUTPsnrFG2bCUyii9sA2YUJVZ96aYo3cRgyEZ%2BM%2BXDASdmCDyoaJqhev18VBggllrUzAzyTXMY%2BoiwRa%2Fdk0GtyBAUFxYpVhZwLUlWmPs6FChqTyZZCwl3LUNXSuXIYDnCc2sr4pdXXqSN2N%2BBjO5tJ7Pz9kC1nM8e%2BJXXJVteKA6p2QY0odvNG%2BwOmwcDuHgTL%2BAzcdgI6bEJiF8kvPmupyP96wOywv%2BexniLwPKUXlFcFOZwZS0bqmVF1nTT1f7Jlca7NRdkIohCwNtSwjuQN1uDCcBO8GJZTDshbCbhCzX5%2B%2FaQxZa%2FYJTRNzI%2F85TkybhRJIeMAEOMmv1WiFXa3q01tmWzD8m%2FfGBjqkAVrXI%2FPizW%2FfpFVY4Tp1QdgvQmMJoxzW%2BV5%2Bxyci%2FTDpAdBR%2B25O%2FdesMVAZuyTzwleChegnhCtLJ81sD92sdsUEx%2Bom8thWIjKuzCRMMfFmmh8R3r0mwcLwVa1F3g5NKl227iNOnHoLgGMnwYDCbbZ%2FuReiw8JtG9hb8ENynL7%2F8u329t7xfIK35y3hKGANWAEnDqF0BZrOXKfIO9PMnNRhpCbm&X-Amz-Signature=5ec5e203ddfe7022f6cfb29a989a4bf64a39374dd817a9de7c2b463e002a2e49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

