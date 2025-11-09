---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VWDH2FV%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQCbBQT1lPyRxen6eRKcvI%2BXSUXKeX7ORXDubz8NJIW%2BFQIhAMBwJPUygfGzSxBVaqnAK4Xu%2FNWeKJ%2BeHh3ro24bBseRKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIE0KyizJcLAOZYKcq3APzd%2BoyDQCeIAcXyCTKE1AcdzYhy2CQN2iFW4qwCPhZpNrmlFp6Ry931T5RGpGyNm5uNL%2BvR9MtjxtzadScBmS1sszJj0J3CJgRu9kvkNsC5l%2FRF%2F5L1nAr733ExaydgVWkZzIrh1I1lKAC4Qu3ThbPB5BZF5O7M3xH3E%2BwVX%2F1yT0b6hvDdRNEgernmz81suVXrLY21AYDQD7dFADgvz92wqskVLjokLIjJyo5u5ELX5guotP2Ih7YBIAR2LuATCOdJdWF022hHqLE%2FWXggHoOMt9iShXPrFQEtA9DaYL3%2FaqUDg0c1R6TTnWz1SQQY9KG5w93KRfS9cdtoTw9wOZryLBKa5LYAFxjrPzI7UZzLLhjkdeJil6WJ9sDPW8vgCyf4Q6uQOroX1EHfS6SzAhZ7OtcHLsMhhItGeS3eTpnZRxMYeuz5tceUQXggCC4SIgNrvDwprrd2I6u26sNOeI9JCkNpt0ZEE6CsMurh2CKbA7aCdggLOy5dv71NJP9lJadx6nllcvHpvzNzDVd0LBU0MoqzVM2gmvLdKnIozwfxx%2BREjkG%2FmDo1iC7xfXSjH3%2FNp11KmA1pfkyvl8FZ8FoKA5tr7Ey1WQ3SEHS3prdjXROmSCE5op%2BkP2q7DDrpMTIBjqkARqJWT2m7nsnDex1PhS9ZC8177sd%2BVb3Bp3owaMLfsQRlXm1kkKwpcgU6DDGFggOmCPn%2F8nxnD2iMUkBNcczcxabCVQJGUhRb70HaK54L8G45DhsnHrmi8zRZuxHWWWypTM20ACz5UMHLK5fzWr9fxmpw11kdqSUcDxS7hNfnDw68ncNIcd%2BnzdCT%2Fnud8kELZQSQBZkhBfaCqIk6qJb4bzcPl3P&X-Amz-Signature=10c8dfa2bd35d49adbd18cc53abe2028b957e4de4f0715ff07f3fd9021ac6d8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

