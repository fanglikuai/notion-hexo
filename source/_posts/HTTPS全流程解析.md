---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VD3FWWOH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGrb%2B5gEtNQsU2TYI7JTsXDq6wLe43QAvE%2Fi1qwIoaq2AiEAluoHwtknOcCgCwuH7o4RqEPqll5nVvgtgAOmnJV0EuMq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLMQS1U3%2BHBWPbh6vyrcAyeOYVjVlCnGiVndHrPcE84TYtQtwHzdqCDs4XrC3tnUbW8advh4WAtNUbKEp4b09zn1qeq1m3vcAjwhE8d1pIrRxz1UHVRWDLLAG0yuWNatlEn5GY8NrdywZsLNYnZRmdNxotYrScNnJhtG5nUb%2FSTC7aT%2BiEDiZNoYzf5IEsYHr1NyytMlMjwUknxC0%2FpnKxNyGXE7CWV7pmeNyLRjVvU2hEZJz6TIpU8Jo7UlhTm789HBnEGvZWVZJOID6dy0tqveyPW%2FmIbjhTRNRx%2Fx2vb5TSVQ4IJy6Ft6k4mdVDkfPh921J%2FmT5dx10Ei9y8ozE3Fdlysuu6Sh%2Bjt8TAuE5jF9JgcnECtYujPrSlGAl9IjfYxpC54SM8xc7Ju6jon58M4%2BsOd2Ry%2BOvxFBk0MKYEBSu3HK8nhXaFTFJqQYb1UhO9u3jHMmviyuaAuoAQt7N41R8guZhQ44v313lpf%2FC1iTxijOl6as4%2BZCwxm6ID43UftLIFomLsOgAnqxGV5jlxjzyO%2BjvbWFlFZCJ5z5kA7SkO34kEnSW%2BnyT91EURsUVNCIDjvOFSi4CnPmpLFmG48c0AOouow4xkHeWzw1tlOuDBlgDVB62ZSVM58jFcJpbbEKseI0Q7XaoAnMLvy3sgGOqUBLeQBFPRGQ4cZFZP3T%2BrDJx6iCYiHA%2FigJDwORsTc2iIkoPzTT1h3iQoaoCdX82nxQ5z6l%2F78d5fvpAt2I%2Bc2n%2FFCA3yldKF2JaO16iYzWCBrickkQYAFVitqIfzAmnhfdua2m51jqGD5TdPUd5k1zISOJ4XC16ILQTaA0vmoVm5aBB9NO7pFXno6btPbo6ZRJKP8F%2Bfk%2BKKXb%2FC1SjKOoEtbE%2FaA&X-Amz-Signature=1a3524795ab30af118abcd726864ce335074fe78004c446fe9e80bd490ab6e82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

