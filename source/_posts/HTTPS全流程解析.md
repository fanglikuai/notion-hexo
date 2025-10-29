---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663M2SFTUR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCyy%2Fak7GVdkZrPBs%2FkNis46aY42M2MsUeip9n0ECOneQIhAIvKzKKLDNeEx8SMnFlPfxe%2FspKntt7eAFuxUkiVUMSaKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyZRDg9Bzd52g9xMAwq3AMH98JWxsX4nuo45LiLxo7FCgCCChl6GqBeoMKPUK%2F5tkJYJWn%2BbT0v3bSbBHXarEp0I2Zk7BEFWhlQ5DMGrlEmOBzeBUtdS6OE5sArdXuJIbXrnqoFfBCgqrVMYyth2MAf4bVCTWc1QMN8t1JaESDZQts3etGd5M%2BvAEwkzzMnTqBk0kt6UAnlnCZBa%2FREjeJ3ZvOJrAgh60etcAa%2FAktjNqapQ3j4c5HQtwcO1IOC5cLIVJ%2Bq9fTZAM1PP7h1TIkdT6n5F6SrzMmjNybDZ4hiSNxBS4F4ZvsCBWcFu3z5N%2BHFB1fR1tPrCWlXjwPSicc1FLBoH0vP2p8FbnNkcH8tSjr3U7r7V3xE6cE%2FVLHM3iwL3DgafI3uIVlYOi8WCJX5iESnepe2cewlJj1tMhPV%2BdOenJHijHHXZCED5JkuEItyx8VhXm6QpWTgEqynPGNQOZ2Gu2tH2ikb2jjs%2BxvnUVqeqX5e6GxtnWTH7Rn0MaTpOn9zcsU2KKVqHrokBBc65n6rk3AoBWyN4vQ0l4ijOoV5eZda6x473OiY5nb5oUFxKAS8W2FvCXRch7sIRBT0KWz0X8iuj5g8h%2F%2B%2BAfYIrU3qTjKrlu4ECTE%2Fo9QBc3ERng1RkuKYetICVDC%2Bh4jIBjqkAa38q%2BiPuqlimf6IFPnWt3BICXOb7MaN6e7M27Vsp0bxC08jHv3caNNZPi9SWwdCLKe2nHogXHPZdtsPuWf7AOYsPmVkMtPTFL%2Fep8Ue2p5Z4cV%2F4i%2F39DpOwlW%2B1NRdMYK6dQCYv0o8y3lgK75OsO19j%2BKLJyICs6O3aXS%2Fj0lXT7EHp54yQKFyzCtkUr4FBi4Q0WD4bVP7DCLqIQRIgdvjjEP%2F&X-Amz-Signature=269de5101c642366a873c5669160ed31cb4ac3e446383f2e0dbbda47ddf2781b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

