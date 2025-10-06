---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOV26MXE%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA0j5wp8aHoIjO54INl492w2EPuCkWu%2F6baM4iXreUH3AiBcxII1LFrxgOTo75%2FTwWKy35n9P%2B%2Fhi%2Blph9VfD5thMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNQfPYMfv%2Bj%2Bq0TKoKtwDAw7xGOK11FLixiZKqhuLPyC7eI7M1bs3I3qO3LQBQFnY1HjDGcFHFwQ5igKaNpVY%2FhHos0HhfyO5VH9FUa0ByWL9ZDSxdnSh3o%2FMqng1NoEvptVa0qgpZXF2dvjzVp%2Fu%2FHMRRNcyFUHr0gHdHFneWF6Hl0vc6ktXlte0wQTIxyQBAWYy2kCFqPt%2BUB%2FbZ%2FXMw36LDG%2BTKWmKJivBb8ta%2FtzNZyu93KYQe1stOMRMBygt2CAvlKNk501m46CRCyLPg9CxqjmqU3t5%2FhdnOAHoCMBThX1%2FOHwkobQ9zSdeE76LojutGJYoh83lqZpOS1LWVuNyUpwUrVZS%2B7qjO4toQf2A6dPi5gY%2FWccGpAvIWzt%2Fy4%2BlfM6HDuWivlxji1ex3gwT4Hp4t0jZlYerNr8Yedv3gzKqZx53KIDT0oGmAR3s3I9AX2UaPDwyPvnqbZv%2FPseeZ%2FgzAvXLOIACIBP3wuJQJCk1JNAgZqlgH3LlKUwjpynMALSlaDOZOK3qkif%2Fb0kN70B2Cjm2p8bfkz1fi6RxOH49sjWqbV%2B%2Fx%2BxX%2BTjquKbNIGCFnjLgl%2B%2FGA9zZ70jwu%2BSVSdu74IPo5hZNKtqHjGXv2XzSgSMtKWebvH%2FnWXjTrC%2FKoV8lS9Mw99CPxwY6pgGOIoLkNkOPXBkqc42WsrtaWsMEwILsrm70SVsmYW2KPYm7fsX9iFFlYkW3CErdVpZNJ0uSlIWfITeO2%2BjPMQNUErScAQaxeH9vNWzXYyBB8mclGJA3nU3rCUHaXym9lI3HigDr0WcU%2FfOU7SiEvxl%2Fl%2FdVRAodtxpMBWg308AWpwGZJF1n6AeVXVV4Vf68r%2BQkmtIw1phPrKaDNP44X798laQ2ymby&X-Amz-Signature=aafb56dd2fdbf37c43e1779aa9f0ddaed74db422ab1193e82c5f987e5187a07d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

