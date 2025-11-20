---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VC4J4R44%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQC%2BU%2BuoAhejYhQ1NLUl2ydVg0ZjLPL42e2RDB8865ysswIgPx7Ltrtmyn4BwH23TO39s4TV1RQYcIgxLfuZttHJVI8qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKwOq2VtZlxU9WCw7SrcA3Dczt7oCkq5aSRON1nX8yhv18XrFNrEydO3FPgQZjTFPeybBH0yFyLdOI%2BZAYumleBZAwfbUS84GLjxnNNe6IFvsyP6ayj3YUjLTY8ExyojsXSZaUo732N1iIU2Tkpr8ku1cBvADLsJShpvfwtClpV4M2J8EAOJ6DNuWltM6luNMhk4Ke%2BeSIe4RIK7Jg7uuUiuoTbpJs%2FauzQj0n81TE0WwVlTyyPlMhgzC%2F%2FZ2fyEvRbiRI1ADHr7hAQrCOsI55vOxrvE4MF7nXEBI2NcgXPd%2Ft9HjahVkU9u9cauotJ%2Ftr1%2BH7PZ0G%2FNJoLHdeMRL68sb0NqH8McHW%2BArtF%2BwwgZPCRd39vUJ1uAANiPEio52%2B%2B1uySWIrb%2FYwCT1G7oZzpXTLwdznu%2Fc7c5KW8WQlgUfIDxr97j3zjqx8XkbtWgnxxI5BbQVjigkpMZEg9ugDL60tuH4ygwPEX6X5jBxeuJnCpKDNaej3bGEKjvBHVaiAyfUA6XS9K75Hvjxln4qh2bvni%2BXb4a4fFKpxh15JVuXoVyBYVJz0Tqr%2B%2BkSkVVbwwbuylQjwrOJA9y9O60QrTYballsk%2Bzfk9dtDZLQm%2Bghs6Dwt7MnE%2BxqCPgO7TvtOrhfJ9CgH42Q6WRMPeS%2B8gGOqUB9lN9wO6VBrCudpzN3cPB%2FzMmkfW4CTnpG%2Ft33XWZeAh55O4aWRNiP76nuKDB%2FU99QGkrTC6XjnAbRwTuSxYfVDKgPCxnl3c%2FUm23GsoASl6oPehR30YGm%2B7wwXMusEtKOblBk4J2Bul041ga%2BTwqAaiyodEV9QBvfkhc%2FjSOkM8NJv9Fw8224uKeUp3w0Mq%2BYcZ%2FjwFW9sZBtGD0tvEn%2FrzT1BTk&X-Amz-Signature=7d3c4985d7c7074b7e4e0a33cbb5c88b23c35bfb8f602431973c3726e62a8551&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

