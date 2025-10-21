---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZSGLFYE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQDmwfW0d%2BjI3dTYXgtECfDlNX%2Fk5sLDlO%2BuqY4TC0WkmgIhAJ4IE3bnABWecfKKkyCc%2FzZRSKG%2BBpyWDYLjfltfTLW5Kv8DCBYQABoMNjM3NDIzMTgzODA1IgxObz8JREjfLsKGSDgq3AMHxTQukSlRLbdsXZUP645Z3v9vHnJw53tbf%2BUyVd9LGxvZEgqxe53NvrsjOElkTmaZYrN5W6RUonLEfYkU8lxz5qLJrAEVvX4hKzBGZGyH1xmRPUXybxc6yYy%2B7WFiA%2BkBNI4Epp%2B7jNgtgcI83vE6n386VUcBc8zI2kO3n%2Fa%2BnvfIm%2BBxdTpCiOfBCaqFN6UpfZEgEC73f7ZNxqCuImsKQUX24jDBmG%2F4WEZX07HTjvT2kIjEAOEGwEO3Lm%2FFD9xfGviw2xNNwwkgIlIZ1RcFt2tXAqVYU5klHARgIbnl6m03UzX6b4n4nd5Yc2uwb5a9o3tuVO%2BBoIgBTyR4QDrobd0N%2BSi4IIKhoRQRYugqW0NnkOFbpmGGhXJw8VItFuO9oI%2BgiFcfkES1b2vG16M%2FM%2BhX6CLCPLXtYcVJHO7ADt9o92Wjbqe9O4u7y%2F5sH08zq22BQawBwR7rsszwY5bmGkVn6Zwm03MM0UxcTlkoBG4Iiraf4SmDf1jCBMZV88NXcBBPuQgriNB8FUk1B63dJLuM1Ago7xZbxw6ppEpZnxiZX%2F0JroUI7jNNmPEH%2BBA1x2gtPRFWvebbnofynGfJ6E8t1S7YkCpTUlNt4i%2FdSHe0Z085Ok61%2FPNkHTCN%2BN3HBjqkASIBIHiMwwJ2Y%2Bq02MvCo210LtldMvDklP5nu%2BDD2kgelBXwmwS76KXr2btOzk3E3Syvc%2BRWGVmeiTpzaoVQAJqP8z5p7NkqJvMx2tbkuPL%2Bf%2FAKjIqgLeSE8UJuX8toZuPWBzvUazHTffvI0gFrww507uRKFXqXdZYiTqzIr08imzJDvHbo%2BzGLjvCgKHCCrQiAOn9AcZxCgR40G6OyG%2Bf8FPS9&X-Amz-Signature=5744f4f6e211ba0f1a500c9eb317857c2cee1d38701cd35579dac4ba8e1ac846&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

