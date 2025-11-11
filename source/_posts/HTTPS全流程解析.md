---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMHWRFFD%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCICJe1lfU%2F2MCYSLLgF0mIwN%2Fn%2FaPKG0EYZwdDqPFTDaJAiEA5Pi4Sg37uCB1e1lRGnwWdtrboafq7pJcTwHANgwb4Zsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDI16lTsP25%2F5ukOe%2ByrcA2gOY2lJLqNQcnD7L2D%2FXMon522ZuXRZ03gOW9asWMOJlqLTTsex9XtU0uzdeZiiTqj9y2Nkltnvht14q2wPe7UYLzqFMqK5DSCHJwo%2FASZw%2Fp%2B4gVlAQ09bOkbmPM%2F7EQnO40Xkw8%2FmqCez6r4izY%2B7NAr%2FkcX3Y2o1w%2FdiVSll7TvX3Fk8nyGy3M4mYQ29wY3N1GhIQIfKWrw%2BRSPOVGdFbnlX2u6kXHV9BeNLVOJn0FHpls8ZpzI1zDTclbSAwiFICsvo%2FCX0G0SCEWY1KNAs05KGjvXNxilDDhENAQXrKuaik8g722%2B4LZLUUrjLZ%2B%2BHjjzaS5TO9uEIvyuJNUapy1Hyx60F%2BM9oCEtoFKOZMMG8SA9vjhmGlqbXPJpdeTjFsdgi5gTbf8jcIeJVG8tAb2AMk7XKH0APON0u7VBMgenNzls33WwvAOG5JjDgcNh5YUavnnd4ZJIcfkNbxggpGLKwKhr5uiIFWvAaG4Gyb%2FaQyaXiQI2hfUEvfB5eY1yHAaAIKhYptsfrs8DJ9vnmg2xpyeW%2FSU2l%2BMJKZEWabT9AbfQ7U9ldpH2FG9pZlt%2BLtKhyhtUUKUzo7PqpGLGQnF%2FqTEWkSegvQfW%2FyPlU31g9DwQP2Igv1jQHMKHIzsgGOqUBGFxhHAKh3We7tSB9YSYhGuF2Ghtll59Z1%2BjNhgtc%2FGcp4j29JQneIxD3Y4B4HQ8jQ4NYnjbuNdYsXXLXXVSI9z%2B2kLgNARMQzVaxNNfWBJikyGbaG%2FESbURYmdkZZU26%2BVBTDr9zSPhpr6EaLZ8lLuQNmdh4TojlS2ff8gm%2B6XA6KVSiT8s1VObmRGV4kWR3uhXK3FXa56pG%2BkwTa5OPr%2F82hEnz&X-Amz-Signature=7086b1d94c09d002a0e507cb46ff65db90b3bd011efc1b0b33b9e3da79c398b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

