---
categories: 整理输出
tags:
  - https
sticky: 50
description: ''
permalink: ''
title: HTTPS全流程解析
date: '2025-09-14 16:21:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/8138b291-dced-4d31-8bce-83cbc5e067af/wallhaven-3ldkoy.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJWBZ7QO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGlUh9F%2F%2F9UfsmwQQsX85df%2BscpOyX2fUsLY8J8rRIq1AiEAsz5DBgacCRL2oupMh2QJ4TtFUahlsEHwlM6LkWMBGuUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCgE%2BWnlPtYhpKEnrSrcAwC0J%2BVL9ciNn1%2B%2FDdOKZZ5s5l0EQMz%2FusbPdul77yJPMnsclN3gdr6Iirl6yJ9RPnSwVH45pd66aP3Z87eJ9oytXaiPsoThT44IqaYryCDjKU4REABQF6oro7EepNxpddbg4RhVKJ6lBs5dzmH4ps0M7MCL9pMeQPmWHnkccczVxzVMtaRI8durCWjp3jQ0%2FLPebqWmSHQTwqszGxQ6rlmic0Xxf4xHk46YNZ7LLfqRVpuPCprFbUFvRYLp6J4cbkrvwak%2FDqyu2ac9QoCIx3Jkx2Mu8%2Bv%2BHhBBf31LsWqd5HJD7qOp1GMzeavMXKCjoSZgS%2BYm75fY%2BEfO2Awh7q4lX3RfxVHhNAttSzRsiG%2BbVrl5N4S5NTz9Ar228keqZYKnKLZh5v0PK4JoL4jTFgdLQE%2Fk33HekakwgwEU91OR6xM29YQFvJ2yKZb0Dum%2Ff6ZIDOwj1P%2B%2Fafs7CyDrAinQWCiChZJUjTe09lK0CVDeEXVXo16w5oT2lGpFXTVb85BpIZefCGwpXAPcWYQxgyr9VO%2F%2BWXilWduJpU%2Bs4JawYzZNKWmAY6Jczb5OvOurLQyxmBnFrvugBLg20jNAIugzARJqSc453earVvCTGVp%2FbVx1KP0yLWzDfEJcMJzO5MgGOqUBZf2eq3w8cLBk6uQ%2F1vr7aAixydfekop%2FBuUClMkdBP%2FCUUFLwN27yU7MCLm17mxWDGoKjEeGNHJ4sVAl%2F%2F26hxeVyzDa2%2F%2FurcA4l75OqbxzlmlqmCh2gCrQZ8yNmX2l4WeqvPZNJHiNByE1LambW9beN66TMgoKq52DB8MQwQtEV1WiTUu7U9uZ%2BZ%2FGqZgYC79Oxz1Ju4geaQj%2BrXJyaW4VWeuM&X-Amz-Signature=579daba0a21cb69aaa01ee03c3a848926110f12064c1252a8250c5513eb9a9ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

