---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDYBDT2B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiAEKH%2BisnxWTur426eOIZZVLr6U0B7bnwgrNApjg50wIhAIcFj649Zkxb5soXv35lAtaSlqu%2FN7c1ySMWUyQfsOkpKv8DCDwQABoMNjM3NDIzMTgzODA1IgwpKqV4XNhyKNv9NiUq3APPCNgG3YeFvEkqoWA%2FuNPlox%2Fcslrv6Q0QYPBQHyXbNpkRbNYudwZJ6%2F99Xc5NQmLxYzINJlThVH8l59qlmpJqbhCcrlOHAdDa0ZoNOu4q0hrXRfuhEj9l%2FMvaeMTgG6a0RjlyaOwHf7M76jHV6ZunqLMDt0249zDCgsznC0NHlE4QsbuEutDHo1NmdokOtBqJtUbMD7nhnWWNdzUUx%2F3joVikPMMVgebfsS5tc8CV1aPR%2F94SNPeL0Dfpuh5Fn9mwFuXcO71psK3r7GAoII1xlMz2Huw0YAmFZhQOMgxQyxCRsfEeRakh4SVhGd57tzDRHRjX7JTij3%2FUK1wsytZYGRzX3LkU2ywJpyXHvQ5ndAYddN7oqCHcKRPORn6%2F1RiUMvMzTbIl3zrJGO1ThdlTHoau4RCUxkvL5oy4t2kHpXNA7IaguMTLjR2Cehb8HYf%2BREm2gdZvmGzIW9RJEE3dRbu3AIwrYyroRZmuDisBxoPhn8JcO7W4oTR85ilqopdMVeAUgIgtHXd8dSRks%2BMua%2BJnkroC1KjXQRi3mu%2F%2FTkc4i7hqveWESf0%2FuIF4ck%2FZePy0GEpCyoZlYLqSQTGfK9QLWbcPd0%2BKNFJGB%2BwT45L6xUyXHabHwMZ%2B7zCQ07HHBjqkAe%2FR8RqbtOAd9P15%2BDMcKf4iMv1u8JHZ8CGS9uqGpBmceixLrz8g2DBlj9XLvKBQnWqP0RsmQp0StFlq33770CFkwSUFDGNgoqDXnprQg6pSxxosMY%2BPWcc9vGc43ZQLdp03VnTRYPgIGeXh82JOWp5RebpWLrl1WzvfaATMiLmXzqUcftrnZhli386IxeMqlUojbzBSkhqrXjKTrn6b1hioJ9xw&X-Amz-Signature=0829730d2429b7c174059dcdfca2db657e0dd756df4f547112b59cfebfa16a5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

