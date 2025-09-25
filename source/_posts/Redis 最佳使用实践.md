---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YHHYKYR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEWQjeQ0nRndFkXofnupw0PVDKBY4n1qH1Kut2ga9NwAiEAqoVtMVc3BwSVAdRlP2rlyBmo0bWAaA%2FETIMKBIqKk9Aq%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDIhuHWyTxznX5LQwiircA0PXztK8xQdtxzVsn4KIGAqDTdDXnpC86Pn5rRp4P26%2BYIgMIGi4wbzvWKrWrPGXgrbjyib0Wsw6VTCOaLcyHVoSGXEXyzSBMSywqT1Dvkq%2B9la924zAfWSSx8t0hQzrnW6pAidGPHfvIih6b3w3BDhogYBAiLxzxMy4pcdgMftJ%2F%2Bf08EGp4oXCSJgpeUBxlafmXT5%2FWipVT69YPCRILoF8WiH5kwrsW3ZLg4fJyr%2FnMhf806YmOi%2FTIRKbgk5Zva4VXiA7rc5VvwYD69eveDslGpOr4KODrPx%2Bx2VeHfc7YLE7p%2BCn3Cm7QLkccqzybbbRfpgUeORVHtCammeQl036KnZIIJsb9kVDebx1Ybzw5O9SxcjJxA1rUEaogBuW8J2U1LyvR327blQQO7lk%2F%2Fwu7ybVUFsWXdU13x4wCjzRPPeUNDq3htvk%2FgaL3vrG%2Fat9bsHy%2FA7YdzuimaMkFk15ZdQIFXydi14wxlnW2NOcsEsU0OQrJpcg%2Bnwl9h7iPOvpDgBRkIsZMsckbjNg1vSN1th63cc9A38FizbokvE1eOnnTlC64bemMCjIiJ0nmKEOixbzm%2B%2BhN2qvoXW1hE8Vo1WTaQyI4UxHHdQ6EHUPTHyMKqtBWsjYUsX%2FMIqp1sYGOqUBj1RR%2Fj%2FM4r7QofhsgTG64lCOh7f6xa9%2Fvwsg8QdNUVjLOTSOxheFWvr9ayqBBvUvhoUjqsw0VLDgH%2F9UO%2F%2BEXwlVK%2FlfqnImnZ9RzZWu6zRx%2Bs93i5e9TeYddvcbNLo0Yg0l3x00RAd1DDef6EyGKNEXpw7ZjbV928C2kza0TS3BKWc8XMAyL8G1V%2F0VaX4FjQO2lkA2Hq%2FZSDyWmAlmCA2kZLNQ&X-Amz-Signature=5046559d2c6df57d61ccab34ac57c949ef15fb8892c6b1939ff4aba9cc115480&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

