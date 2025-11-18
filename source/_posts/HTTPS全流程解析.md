---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RPJXX3R%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAmHF4JyVoRle4jvqCEVsKdlbYxGAvjrOJ3E6fkZW9iTAiAYVX9f5G3W6f%2FnzLjlUU9qBo4L65yzLrGOn24yKdOm2iqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FVzJbdHfUAHJE8WIKtwDOuiJd8XleoMEJU0ElIn%2FYtF4vgrA1RMwBD7OouGGcaKnshngOOXptMsKAOLW6FJmo8yUV0I7cSqCOF5ExGpD7HCDRUtyfVqseyrdqAFKKUP3hUi%2BVLFHZmpSG6JmPmYNOOfYAZQ33nKfEiuIN6Fdehx9JhzosdNJSaWAbDzY1MxCN5G8cx9VwKToEQ2wfp0JsmlIH0c5Zp8sCPBb8nqFh4S7N97Q3rXDEoATIbK9W9pBdNINq%2Fw3SjuAjzaIYT374zraP0vbZLo1An2EJHbyplP3smC3gL2JynYYE6vd5ISrfYrC0ImULMtKAEuLJquCaWCh3rCfEvF%2BfOd0PH0C4twPXO9a%2BQK8bnVPYPs7UjyHiDsNDbiy%2Bme1mAzeZME8dLWWG949gt1cIShQ3WkqbFlcv7xio1LP%2Bo%2BW%2Bsy7gbPSo4hfs96bGkY0pP6lDq6mcFkLHX3EdyJNlJFUibE6ZaZ6pamkhiwWEn0PiCCEtfeHV5nB5pXlOYgeQBGiBch8jXx6K8TL9b4sh%2BVtQSSyVnUxQfYluhmVW1%2FjjoTxc7FeFjT0aMxPf9YFuOdqh%2BfKpd9BZPmGcsSISUf7qrODE4kYFQfQc6TJE%2F7WVWgswASI%2FvC6%2Fomd9MrsBa0w6MTxyAY6pgHDYy7cHLbFRb2DB%2FreyLrDiCFi%2BLu36tZvhNFC1jS%2BLkIWl4qKlY5ZyYuOlQquNbqX5U4S0HcyoWpYoUZNfbUAiPO3o0e%2FIUa4mvFzUxqnqX1AIWIkEBi1vYxyMCE9IsGYIAGdvippUZc2NoofpbpnVNjKUbxNGNu1R33d5tYX5jof8tdxOMB78C%2FJ8Bpt%2FdZYhaP%2F6cKB0nawxXKcnEfSP%2B3s5n3B&X-Amz-Signature=065a386bac45fab51cb4d8eff93825322032ac564261cabe7e5786f9e01d1fb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

