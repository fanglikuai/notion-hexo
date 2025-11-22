---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7QPXNSQ%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCjfLtTQmEL%2FtWjENEasqR%2FKzHG5nmKpmZY3%2BAbBEP4AAIgHPCz3HwVnSw7Zhy2pK5M%2F9blES6bIQQDXqUtcs5CU4Qq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDMay%2FVbbvIrW4PtlCrcAzXYxyp68Tw6dUqdSY8h9RR7fycswP7YvmQOeT0ZMWOwJp8GvkBHczoCiVqUwzFj5%2FReSQJ%2BpCeDVUDjMkhOQACZoJ14yAyvSfdT5VnwBl4%2BCg82Z0SKYiKY6%2BuP9kBQvbyQuDrsiWo08y%2FhBmkBzRlVi%2BjXBcoXYMRkmfY6nCCdv9i%2FDWvLQA56WDGk5%2BelU4tkDNsvRhSaFRQOjvsqcXvae1VKoXnK7bENLv7W4CD5j5hZw0V2lNjmeCNIk%2FmUOoDxiO6goWUGVDp2Zyxn5IipXPghCdw%2F5KyXUzDpRl9tJTA9Pwf8%2BQ%2FwKoFIsBM2mk5R%2BkqUNZigqHyqtUwFVS5w5Ln3UywuwebmCbExhrWtvpva2wFMN3CldmeDorWBg5SfF9CCOB6kBHbO1fWBhVqLj21qVDntc5Zdnnhu2JNNT%2Flv9n94GJZnPrtcQ7XZDbFP5CeOPw5WGgYjvhUJBA6UMSMilyZHLrlYrt78y4ws3VWiVVMbBgsJeiaFnjOi6ZK3mZkft1usoXpl0RRJPq%2BVrxGQdmLX1%2BFFlEfBRjOP1FdSx0bDVUtAbKZ0XjMeYKmvxdizZtcNEpyW9e6ThLfCK3cHMWJ260%2FLnpJ%2FGKU9T4aDsWzX3nOCO0JTMLqjhckGOqUB0Bz4mD8zYkRQE%2BrJ%2FDCcJcBR7hoYoS8JX3te8q5lF6CKZ5N03GyllIaMTNH5eBdtfcXuGJu8RXuM6Dmp7Y2sk1HLVG2FPb6fhH0%2F3x087rZYp%2BebglkbR7%2FThE2KUBauJrDO53GYTtfoO6OlR0dgkoGq4lIA4ZMVsCa5ti4euT2tgoBlVZMyWRoLFVeihdWTrz33aPi%2BaoMquqEbB22C6d9UAItK&X-Amz-Signature=7361a1c48be28ba7820908e87f61be8b5dbaec53049655d4e48fc8e7a0640fc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

