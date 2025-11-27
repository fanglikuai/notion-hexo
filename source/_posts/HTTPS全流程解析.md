---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVDEQ3NP%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5CDOGyI4Xuo4mNFhgGn7KDls8miihkH1aXYLGYF62LQIgB52qVgHmdD%2BN2z5U83ZCCTmLoaTSfjp%2FvOCh49P8Y6MqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNg4XObdxUkc1uqj9CrcA7aU28Nww0vLtq%2B7JsvYj%2BC1RTl8A4U8JqpP5j8%2FQUtrg8mOj5m2ZNH2TG86Rl9xtr%2BbDmxP2qGwQ%2Bm4CFR0LT%2B%2BhhUzdS5DVFuTj8peMR%2BuBqj9K65Km%2F1xCKLJf%2FO1o2fxmHqLv0F9ZMBwWehTKtsCErOeJojEUYNyxYraV3EmhFl7UUYVep6pzXBvGOUGyv4eom67J%2FcXYFbY4NuPqT2JBB5gMjDB0bHh6Axw4QleB2%2Bd24w4vuxowpISmgsBjvnylluD%2BIFBOqSA15S4SWcm2DauQy%2BKfcUXoyIGSoi0LzxWYqS2qYxJJeBy76YOTyr%2Fn%2F7rWDeP4C8WIDXqESU5utGGDSNFQ1RkYbckabjwFnDADYwfQJK7YXkAV5%2Fb%2BjWLaC5F1xUR1Zl4jnDwllOzw2oZ6Y09MJyUnHiscNtuAvGta%2FTFaBNZWA1OFLow%2F51JM9e0wtcH%2F5iSARiMZftvw9PZkPf1sz6ecfbKB88iTUHvBQ9fMyF1Ojt25qd2rQOO7UvB23AC%2BlHOCS4af2mHCx5BbwnP%2B%2FcDKIRsD8cBsHz1IqKxqBffYlmGxB5L4wsEyuIglaS%2FafnIUyaD0lQvx3MT6gX1JB08vVRK60mM78BDrJO6Y1vD7gxuMOuVnskGOqUBqs9emzfPbYbNGYFwXg8cNl4rUd4plzmfeQBD9HidGTVmuV18TZb3e2VHtOCHs0Cm%2FBbJXH3YPRGy0%2ByN5CIoBsVW5KlHWqpmLFo9JASdOhrRV7wtl6RZTLHFPv%2FjrjpoTdNhcjrDvv3mv47WEnjmoWq7ZNM5elV6QlTefFnKFeg7kUbn0nFPPSvY3ktC3dZsnoXE5C0uBiXMCCQ84P1w7HMkiGAZ&X-Amz-Signature=dbd3585d882e76ad01c59e1709f72c41cfbd164503a4e86b792dee19cace381e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

