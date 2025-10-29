---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WTXK43Q%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIHjqFkSOQMzgX%2Fc9qncM8umQ0yeDQHhulgR1pKsqRdM4AiAXV3isHYLD8Fj79ihth6w8asy8eVT5UgcMBH2uyp1idyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BDrK652v13jYfz4zKtwDK%2FVmB3fTId3iJHSC9gZNoMaf0DZlP5qM2vgB%2FJt0QlT80B9VqpCp3tCIAy%2FFsc0WHNXkLht60WkVcuYITnL4kWarzL5pSU0pvnr8%2B4kqWWprwk17hGz8RME5wIXwVcXZ2h%2FlvNhC%2FmZP0KS1uxYS3Y2JuTCwy6oVTVkQps6BRtkkreKaBopIRgUZT5M1d7TGR5WyQGWRvkb2pe96%2BCdhqAyNdI7oDzRJ1WVLEkaOzqzdbRbs7SokSqQtZ7qa0lV%2Bw9GodHv3dtQPtgh2bcwc7q%2F%2Bf0uqy5VTkfsMP6TC24XI2Gjoij54Hep1HpJHpeQI5O668gtd7UjzE86oGl0rFUxpFr7uomPKl%2Bp%2FGZmHj9D2eOmX%2BF8Yr045UOk4fC4xhUN73LNcbSN4IY0va1WSdGJ0Hj7Jzu1Fax%2F05m7ZaW2FbfW406E1ZBTE%2BE9TQ3xlLBmB8NABRjRq%2FX4edxsL7dL9SfwaiR3TvkK6vBaT1wZL%2FutsCW%2Fmdh6FMgD0zqhnyfMcV4HYxA3i2hH%2FSETvxDiN7pDPIqBQJh3j804HQqza1t9co0WJARGyrt6W4SiYuuidAYcCwXCZ6vTpKOoQ7MpPoCn1tu9oaB%2FDvSWILcKVxKMKnL0nNGmfJwcw0ZOFyAY6pgHxg%2BV8VZ%2BfgWIkcMsbx24Fjx7ztPvjWIDwlL6F8sypKH89jNgpUsOyOWms%2FMwDWe4jZl9yNcBokFXC4YLLGXFPyZZh1S9Hdj%2FfkSy82jZrT8fYfibuQWci1vlP0fOz8maVSk4XN1DK%2F%2FaCsKiWYkTTDssuXl%2FYts7rJwTrmQEL0Ltg%2FSVvU5qY%2FOupbyzGF%2FCoJWCb2bFY08HZZD5ji89JHsWERzvt&X-Amz-Signature=077edeb9233e91263809782ba6e8d3f3e9c2769101448d7a37035da596ecf99d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

