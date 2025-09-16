---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466727NCKGJ%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T160043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIAFvFnyFpIwXUcoQzzSPs2vZCrT6y3VEkw%2BXW58PVDB9AiEAvpIlh17qaNtgJsJWGuI%2B%2FVUcDl%2FrEi2PaSmCIdqvGrQqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHkC8sMtFnJtZskChSrcA08OuxuNTt9m9SnB9he7ihajJFP6OHxfMB0bnE4%2F074dIgQmeePDqsjbzNsjgAWmXyEZk895F3Bllnf%2BKrv4DPi%2FAdchqy9RPqv3ng5D5jR3JTeqbY4qmh25sS4FU67fCfQ%2BEoKsG%2FaxMfhycy3i97vo7ID%2F2NCFLgcfzALXu5JAARTpE0QAy8XSxaE5xedBK1wE3wlNxIqlrIQsyH9OtCNQVGF4nKJP8adR0SYQZzYeoWzHeZ1uIGt%2B8OiMTffcpDTz1BUDxwlsoqQ264jx5zMWmLAlzCOsz6POR4vO8DhT6ZO5y%2FyarPkjO9wkawywCqjBbl7T2Ka%2BXihjAgh9kFsoyGZa5WeggENoQGFZ2y2uzpqVnHn3lYyqWYTqXpDtjOxd8hrj6j8rR2lP4GgDpMQ%2FsYLeCXd4bEEwz00ZSmzPKMhjx8E4PlxOBK%2FqaD4BKP2bVUKggrCK8ILiaLPwgDqxfRs%2FFu6l%2FnZS%2B81SJgmPHlP2atl5Sv5nhoSc9YE5GD1YZiSuR57wMhzIYcjgOzkTXIBd%2B3%2Fi0kgbxFPtXKfEYmgJmTMXBg1EKLBiqIJRtqXEupxJ%2BT6we9eCQcBX3%2B57uF8LDPtInIcH8XXW2jSMC%2B%2BvVujO5YWs6k68MPXppcYGOqUBHs6%2BHsjf8XT84OqnxuwI3Bx2Z0t7NyPet77pYzg%2FkwyObx14OB%2FqA8XHp1i2X3dZVWtH19T51RM9iCwzjOAaUDgHfTcw0IU25f6mhv%2FCuXQv55fopdsRQNcHOK8ivius5aY%2FLR7aHeX54pwpRoog9H%2BMb%2FhSHQcozkrE4ibBintr1zVDc8z2TYhCnQGUZhReVDx5GTumWg3HD4s%2B%2Bzx1TSALBpBM&X-Amz-Signature=6b9a71320de66b95e6eabdecf7539ae40d3842d8d1f771a21e7559aa234bd530&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

