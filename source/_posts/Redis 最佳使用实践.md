---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXKS2KYI%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBetCnxvmxzMm36C69fjX9557MBuSUCBcZZUCLrb4OiuAiAkbioLa%2BqoiX%2F0dKHeLTzscJ35fvGZmUzQsDRDVtjlPSr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMl7JBZsmVfBfrne8vKtwDNz3ls9Ouqr60%2B%2BMc8scTHPgLlaYqMDG0d%2B3AJkL9ZUOi0blUsumgNjL%2B%2B4mtuF1FEFuGilA03zThiyoufvjUiOJQvZSFhvdk%2FteQo6x6ip01pMEGMR%2BhMv9vsAWxYh5a8c1%2FkwliPnIDEHMuujRvG2QPFAXTNe69NtjwLsDGLf9AUmUVRqed2W4migWfxsEMwFVKLgnE8l7cXCG3sJV1QYssmw%2FEFuwY0EAsbzBpDU6pkwModyR9tf9dLlcCdQj%2FN30ur%2BIGbGXCIEwZsCjc4w24St1YA960HFJOz2zMW9dIxNf%2BR7g5S8NCzudSTopDKrnqKXWj98XMw%2BgKYeCaEKmsAj90oz%2FlpRcTlVaDNDKMXtMBfaLEq8eagvd%2BsaS%2FQx1a1iAfP24lCRnk9w%2F51aE3a9NiZzzWznUdA6IVXaN3ztEUEpddcmDnWrwltKRfqJ%2Bt1JdlIWsfOQfbn9UN6TOaaCpqhPd94UY6yErhEQPlWnMSWFzTn69KZU4C8%2FPz4Aj6GO2zaVMXNzE7JWTiw0whSVmlOFR4NBfyl4qGSGoyRbXhN%2Bb8smmNAnZPb9SISz0JHfKrPjiTTEZODJo9dgMQ48UPy1scPtG1TrsLv7i%2F8%2Fu8I3EuOQv9qfcw772eyAY6pgHj%2BN3h2Xd1pYL2AkSV0mY58MIy8zWMF0M0F1jp3jpFSYq9mbfO9ycAXlc6H%2FWE1QRHK4NpRb%2BE%2FjGz1bihc%2FvRCZ8OifFaxaV61FV2IVvBd%2FXfY%2FYBSUelnARcDEVPalQnl3Um1UoZuz0rPaD6we7i7kWKCg9zH7GQG9B6LrGlFU56y7Rt52GX%2BxZ8SK0UMC8%2B0bY3DsySNwZE0eQTLAcsHQsnyQQf&X-Amz-Signature=339aaf1e0e57e2062ec212f44ff07daf56d10be1f61b1c88c1109efeef9ca585&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

