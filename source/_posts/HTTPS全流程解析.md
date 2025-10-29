---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPVR7K7K%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCHHKjwtr%2BuNnZO3%2Fhz9N7BRAo8jTnznlGPywyM2xg3dgIgBNf8BEVtd2biJ4hjfbh5OneMTXRn4FYNViY30YDi31wqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM7TK25QDzpuDtng3CrcA%2FBg%2FPlijp2fFBeEGt%2BDG76tM8mo9sHucJjpfoVYHp1rbMOO4CjpZ%2B8Eu5dKthAFOKe1Af7ayoUd4f1z7Keb4mBpwkNiVUTRxMWFKMghtpAp2SdAjUorPwij%2BRi%2BkJgg9ymonvmlhyR6BImBG0778WgTEyuEplxK1aUBJSel3NZvpWLLCgWw%2BANlK7AekP02b19hRCR0EDVLHEKP7PW3sg5WaijBLbKTaq56Tqeuti2FYnQxWuMWSEkuPM56U23b9auvu3Ad9aZkhSHXKG5sYcbhcnWqC%2F7K%2FHKlGdHFFiV3BFTzPthVVDrhWOLdcHZnkA%2FnlRBe4b9YMQYB2pzEwr%2BpHoKfjdnIEccCy4zrClUx61D2a6xFCx3Hes8Ev93%2BF7zZkq6TjDNzex8Puv6nz39EpSvc%2BFt6UyTJuLdSKTSm5PY8%2FwPuFeMubaLTXh1yCiPB6imiIo2k2DyDeSigTyF8o97Py0Z2yemEOUjOJW%2FUkOKe1OHHT8vTeL8wKtX5Eie%2BSbRZpb%2B5zRMLTQn0mir6HJ98CIhuNDjYfnzMuopB825TraC2DEIpBt47IdQxqu4G7jYD2cNOKo9u6GMO538MTk%2BcFHEJ4w9eKRajhjMFIQPay1Q4HFbou1iXMILKh8gGOqUBcSB7NaI5mb4HuFcZ265zPHZIs1EewyJ5BPO%2B1VX2MOEQZUcWqN3sxLslxrVjgxgbTrGh4PvGsNAtaR97W%2Fp2k2NMymVHOsNYO1cwSlWxUPRjY0e7vCEK7UPZch5%2FT09SbndaH1Pi%2BGtE%2B6Pyz6nB9LuYks0NQg9s5y9AULxL2r787ia2EV5luW%2FWcbzUZtWZlWOjQZzntirgEcsWf1%2BrR4o5cZZV&X-Amz-Signature=48621d348e6ba4402d1874c4de4e886b4f293f84f81a431be20f39e6e4676b77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

