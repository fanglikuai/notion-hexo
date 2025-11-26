---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666INJHI72%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFkclEuKJBSiF5MRbi3V0iGlllkkkYKaHHSKIq8aliUOAiEAvHrI%2BH5Q%2B7GVc4GQ6oZC6rA1Y%2BiVtRqGUNJY8l9chUAq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDA53PoH4DrUx0lgXoCrcAyWZq0YwMBOtw%2F%2FrHGAJm7uQnuG7hwn%2FHUV42zd1%2BeF6nakre0lPfI0NpNP12f9FoN8D02gmO%2BRfGxZLyyLkkrW5l3pmtXzczgVieAo42bO53O9sJI1S1uujUrFiKFmxrexlWRHvXmAH8vzKfs51JoGwyQzQHjKwxQyE4xLpWAovw7HvCnkSSR0VXHh8l1Ag7yLVisj3Tw9DfGBc4by%2BNuhDPbHUoK5uAVW%2BV065sSJh6WdjES6pN6bBRA7mnaXQNWDPSM8yv1ZxoZLLAAerUJxmk1z7IMAde4HliONU0TsRS59Avcm4k2n3w7MbMaxP%2Fbw12TDZbAne8FFyZOl5bNBmRIppb3A%2F0OEsea8P385GNSfDQqjtDwgbFosEm9opHyrA%2FIkSTRZyX1Dd%2FG7MlI%2BnGxP0k5wMbGHo1cZ6GCCfdDColCs0fUDVIaHT%2FGFVjay7fNmXCgrkyPU95pn1ICQzyfJyVcb5rN%2BVBVFY2rTJHfAsIDbcIEwka7ytLb6IxmLOm8QVxmmpKQ3C%2FR3G%2FQgnYWTdJKZMHOD5O60jqSc%2F%2FmADqByKgnodDegPYvD3k6NAc7nChyCdNdm2f5VbrWVHP8HT934bHzNsYAajlx5ao62nXWR%2Bak9ggez4MLiVmckGOqUB4zVptayZsRW1qWu67NbnLcpMUW7y0A%2Bb5O86q8lNigZtIsJp8ZNOvYkwkF1Aj6JSDzaw4vHIeQ9vm0usLE8wIxSFITfQjlL%2FlR4o3UN7WfoxFhSfN3o%2FCebSwJB8PyhfBt%2FcxGjcIq0IVAgXG%2Fip5b5Zwjw8LydVd%2F9S7dTUWYsY7ObbTjKRk8QZvBeIswmXio20ELrdls75jvjaSQxiwpnJ9UyA&X-Amz-Signature=ddf8a2c2c6a5cb8997548f3249945cede998f738058a81b25f8d13271881db3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

