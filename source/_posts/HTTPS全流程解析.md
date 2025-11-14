---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMWZ7ILF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBj9p5U87gMsojIKXaWLpjbqNTc7aBDNbKvcABcfP6uwIgHstwa6f13g6fgZQffa07j4UFJnE1rk%2Bj9fIc%2B9%2BvgwQq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDAx%2BZBi9vmC%2Bd%2BlXeSrcA%2FH07rGW9pLmciF9mPFa6uDmaTmTxjZexrxLDAE5bURJmGIfIBX62TMsMTvHyQb7jzPwIv2tIKDKTsZnHZL20GjmGCcSSznq46dHrnuNeXYH5QQgzI8HBLTo6uzheuRX7JCWvh56IS81Igdq%2Bgb%2BXsJYCPDUlp2ZpLtDmUuJmaOJwm%2BuKYEzCfTbZDAOYoXN9vtYjhrhLdczoTbpiGrUdETIXlf94YQl9MEkKzU1Cjd520QbQj%2BxheKEqDEyLBaj0hv4LKkxN%2BdM%2FfTnZ8i9TWNIPwDtsHQPj7iy06V046qz6Es9wL%2B1lnWUfnrA1y%2BF4f6d0oHGIEF1ZOk6PHRGGJLlJMJbTwh0%2Bzjg1n16qYU5sxDc%2BhlMUEh9R7KeZsPxKbllZPxI%2BKh40Y%2FtGRDZ07PFwlncDJf4wRRy%2B6j%2BmHIGnVWaA3vVIO8d0y9%2FWx0%2FhcVP6S4OfoHU24P7Q5qkBqcIg%2FOVmzIBAFBkYO8qx5i8fEtiU0bylxmY8iEzHVS%2B0znHLUs3nBHB3fUj%2B%2B31R3xZhB4GKN3U1dLfc%2F9VsR5WJx9qjgveX8K359pkgqKWlOYDnINfdQQUBuwc3HNAsyoy0OS6KcNLkq9ZgoTPVmLsH5paV5wO6I6idC5XMJuv3cgGOqUBIp3N0rn0G0jVDLKFQep%2FXO4fIx1Hl9K9hMYDPZrf%2BP56iIQHKBjMkKF6fomd2VyFZylL2uYgxbhhhkESWgq7HzQJ4KOB02eaJOfkQLhoAWOqrmssHHBXQYoJVqq491RIpIuYwYgxd0I%2F%2F7HKISSJsx3Aba2nq7%2FbMAtsRup6EaPGRtgr9WMmDTdW51qhJ%2BTGURopdgD0xB9NGvEsr0K6RBstJwIA&X-Amz-Signature=e2a86e77260e690b0722198e2c3aea89ab8c139c93a4de4c06f4eafb48b56f7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

