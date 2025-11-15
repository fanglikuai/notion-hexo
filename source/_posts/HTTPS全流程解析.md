---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632E5UWOQ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC8u1pi4EXMJVu%2FpCqbfUkh%2BeZSKcj5qZ%2BGc5CzlYVh4AiBzmpayOdKQP2yGjgfVGLWAgPyKpwHnVuZOXoB4BsasXSr%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMUA4M2GKqvp3JhXCZKtwDtZmuZZX%2BjZJyezPKCDcNjtPe%2Fb2dfDusbm5AUpHokfUMDDVwmFzX0nAfbAGy4s36B%2B1AD899ltbK1905B8CXyNEyvIgT0wqsjYuqLWvDpwozOrNAJ%2FxASwyU891dS3P2ZoERPeH2OllmWiF63dJOXibxLneu3YGFRT3ZFx6QaEnX89eE1T9PIU9AMBuoHX5g0gFnYW4bz6n0Ogi1izQrsIc3%2FeE5wi7CNXspTQww0pf%2Fgwny0%2FdQ743nqbeSBh8NfM96Lp9NaHy4JF6%2Bw2F2kwY0mrylrCacWi9hLS1F1HPwOgfk2oQy2%2BLeKgFKSfwxaaLi%2Fpls6ihFvk4TvhmdfDTIM6cEONHUUdX4iCHuVxrIQbND8Z6YookZrTntr3EU9v3eQ%2FHZwhZjKANlNz3jc9Nd%2FFE4FuNPg3T0BW7bsKQGkaNJZ9dPXC39p2kzMWNqj4oMSp%2FV8u%2F9jz%2Fl8yLLaQB2L%2BeYsssxE2y9zlY7h%2Bnj1JT7UpROvVfwq543lFkfLz5e0hYlxkseKl0V%2F0t%2FjV%2Buw0etHJRF8Z7lhvqOP%2FiO0fUHQodGFsh6goGfXMLEGA6rqp2paJRjWxchakzDn%2BdHZtnpguIw2hDT4cqbTieC0ta91FT6sUuxehww5%2BXgyAY6pgFhaPIFCFwdQVaMoldCOGeoYvq8EggfWAczzL9%2FaX5IwfqH1XxGCiDYgDUj%2FIgH%2BHA%2FwSV%2Fceht29UKYZaRIcwEOHHoBfCuRn3WWs1ZhAUfRv%2B8aBrLPEOFo4eu0e5JLSRP3h8JdJXmvNuyahSh6mdui3QFGbrKEr2oafJzr%2FYo5HujL0zXcK5K6SintQLferPtW5PY22szI3QmNt869bcXU1zXznqp&X-Amz-Signature=90244e3facdcfa1c8068c0515e5550c9eae1e8c1470be6f2e5ac58aad62b262d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

