---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWJB6M3S%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJIMEYCIQD2EqmzMyOv9oq%2FDACajNnPlyx5ZsNeU3bJ%2BrLQPku7xwIhAOWWcK9u95rTOLrk1Q2%2FVXQoTkbdegfhkXipz5Wt0vhXKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igya2rFdZ2oUd9R%2BG84q3AOHUdPBSKjoMDfXenMHzgPoQTXbzl0KqHbGL2TNt6D6rld%2FoOTlWVyJAn3dgo%2BFB0WWy04VXiniLdT3KNHof%2FDOWqq1o9KQyYdT4vbFSE5pcS2OKbw2RDcVrFRP8vUM1LstYDHdxWrTqupnLlvcC%2FbnZyELTTvDrPuc%2Fd%2FaPh5w%2FSZQTG192BuEfRkwfUViGttvzpX7KyIlsyYNt7PGdWpEHAmhIN4Q7s4950zKsgVgAWkYN6ReFkzvvZvNXOpxHKe1AnwK7uzu3tajNHQf9UvwHw%2BbOfHC0PtXAEosuXovlioX4WOdPxBKJFDCSCKkJMVQR32Me%2B0EHcgOm5bgiMYKy5sq9EIQuR2%2Bm%2FFoYyalsKXUBgOouAbRTPqeNerRDqApAzTXuodioBGOODeuQi2GsicgjU0K%2Bo40nKZLPSG%2BA96MSZn4pvOdtXe7E8f57SEi3ornsKh715LJIAqx%2FUPWuEai2WsvNukOmR77%2BmRDNHRx6MCVtFKCNSo73M6XQeOVOY0L%2FUv40Fdn%2BHo9IavJ5ay2IlXEEnUzFEE7sG3gPcG5t%2F7v%2FrWlQXO3X5i3P%2B2axQW%2F6Za6CqANP9iOZ9lVuvPr50zwOP2nTZEHub3Ile6qOqfDK7NayAC2jTCbk5DIBjqkAQs727pKWA9OmlE8UFv4MTSDq36lQ6t2hMJimqs%2BMaeUBj1Aa%2BbCPZ%2FXiON8Q3qsUfeZ%2FCRsWzEMqStnrPXOSrjJEwP8kxLyuArwkPUDnmAQ9PKRbuYxc6ogeDdIcnlNZKQhL4NS7M8PKV9W5MX73hEuBHNa8EdSfWsJNhtFsajf3%2F%2BfYxaAL8kvJWJhnHbTsecwCqs1m3jW%2B1ko4NXLLp5QnKbc&X-Amz-Signature=06a1a3024583105f8035b919c3574d5738512a40fe6ac2525c5a0674ac88efd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

