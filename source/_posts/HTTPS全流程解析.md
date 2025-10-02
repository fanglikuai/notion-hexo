---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QL34LJNO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDBr0mDCdxOt8yg8SWF0alHKcbngywBapK2lBtcNAowUgIgIMoNIkgmJ043%2F6LxxdAB90bMBEMAw4CACGqMfq7fiNgq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDECLajrhsLhsv5WAHircA6KAuGvVRYNPfcUgnQvL%2BHsSZLHNnk%2Brkm5tERWpa4slXGNnlJdZ2%2FKjMMD9bGVoxhPD50Utt60GLDwnt0WaxRVPZrDBlnRZbKGSF6FWWRaftZpYcpvk%2Fp320%2BN3eqmzvlPYQ7MU7yvPLP9Heo%2FmMD%2FJkLxxjP314QtAAL6fQD5jlnwfl9DWO1PUfDMe6GKDQh0KhqbgqyIZa8UXKFCO1EAyJG1mhKhhjAvbddLTiXKOYmaEqsAuU88dWKwq4C2AzBKIZpYbhRk%2BzU4734KDcjHpHM3peAZyWZuGw0dpQCofW25HF7q2oXQJZdS4Xnyt5LKE3ecjG8DARwE9MXFx4rZS3rtjX8A1kR5L2Izam7K2AT7hdWEEo%2FrXafl397JG3eG3YJCubLEYhB4c0NkFyj%2BVbztOch7GsPGpCOIapbKp5CYLpdNRkpORP5s4vTABD6uCg794MQUMOwDwcJw9rBD8KRpz6MEHMHCK8Q65DdJneoXlHWiTf5ufkhvIXgX6u6wiBoPouYQhNSv77%2FIG7H9X053T%2FntSnNhJ6QUx%2FNKWWOlDCtzdT7jIj9NaQKHwSrz0sLVCKrlaPLz3BngYG2ZM%2FAzTs2VG3qJzYWqUR93hCux1pwOW1BtXhBbhMN%2Bi%2BMYGOqUBSG0cZ4%2BE3%2BuPHVHpRg0iq1M%2B99IkHQ9hbt2bg%2BBWAkdT0Qa37VEkQ6RGejd3D3pn22vIjvm%2F64yZkNrfNAv9Z%2BVRfDEBFjhQxomhDmK76SuiCjG5W%2BdNSak5OQ5J6e9IeElZVLXeNYFT8zAOdvJzOD5lE%2BJDXy3kPuPKbvf%2BJ8q1p7TKKGgZ9j9GOyJNcmL7GpiILUF%2Bj12d%2Fld6h%2FBzmiSV0GSv&X-Amz-Signature=2f13ef5132ee6a726ce184d12ea140d46f81b7e4ee02940107839435dfef49a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

