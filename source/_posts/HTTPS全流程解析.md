---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCAG6TD%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBQhJTuhuqDmw25kk5CmrqfrBxIqLNMxVA7a5ktIU72BAiEAhBT6UJDF0eLSLWbz1wSMgIAPoh%2BBAIkYsHDV2jfmnp8q%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDAygsP4%2FkpCGvWPplircAyk3s5pKtVg3f4IgXKAJMVWEcrURv%2B%2FyT%2Bp6WZIxkycwGPRrnYUu%2Bme4L6vDcGWgOMOkbxmMbRisfeZK4g9W37ScD%2BBhdnvO8CJrHx3RdFQQ6eHXySPHtmBd5tFnkUAn0c4%2Bk%2BDJlgx6RNH9gTdypU%2FNHDFwq4mQexvaZoyCeY1d7ii0Hp2M2S5zhM1J2jIxE0wWy8vdEDBvhVoXJuKWWSjwcyXVC4xMGWRYpneKhQO1SepsOBBeVaM%2FCEEcZpvBZBQ2ChO7pHFOtpkETFUc0BGdNJTSGk7y0aJQI5bWp4LGEpkRKwL%2FSfM0ZP5XAZ0iXDucu4PnyBuw6vNkGtCw6RrA9Zh4nFJFL2fmaWwLxzd5P1AYnWodPLoNPO6BsRQ1VRqJWDiTVgh7SHaO6LsbY3AieVP228EFEdenRtgLjJpzyyI8rlK9q9l3mH43rjy1jP0AfBHSdHPHXZ1oIMKmjk3w232ZGZu1XLYPMj6d%2F90kJBvV1VrXeJ%2BsKYu01kolrwjtJ9iEiyT01Ch9XcCPeoDHG9mWUl6LKQ%2Bc0AKnClUU%2Bhtq6%2B7og2gS%2FTwgBswu92h5aNl%2B1RSDNOQhaRJ%2BnhtUKq%2F5dLtjaevzOTDvA7azsEJ047zkgnXmufC0MN2DnsgGOqUB7u%2FYkGzJJ0RBjeO%2B86PvP2808SJS5GZy23HMmL0hMY0gtVXF%2BIL%2BwKbzA0prG2ysNAda5MMiiFEKWuAXO0gZ44%2Be7v36yOgF81uTgfJ9TKtsi4om3nqJssre4zQ00O8DgQAD8gxJ%2FaEHlkgC6W3AG3E%2B9%2BE%2B71uypUlTgr2MkprMQs%2BhKIklPrNGnWDycckDhz5mdFsukAZfo5yHVBMn7bj3M9vi&X-Amz-Signature=f2e2ccedf6c2e8a18b221f161078e5f9a2a82578cc79c52bd3b4b840a0e6a394&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

