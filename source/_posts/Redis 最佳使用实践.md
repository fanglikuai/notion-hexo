---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2GTEQOK%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIBy%2FUS6kJr6EfZgN7hB4c2pjuzy0TVQN8iIcKVzWC3LcAiBnWZqcq%2BEls6W%2BNR1Pwf1CXh4H0%2BkUP8BOuj%2FSUSgKSSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDycghE%2BkQUrda45JKtwDicsfEF%2FAoMruYMqDOnyznTatTb4udYiDYnl0sXunD1TzxmeDU%2Fad%2BgyJRjaIZg3kLbX6zzU5cOJA4PdsbYBUOSJBnuE3GJ8AgKiAy50rVeESLDf6%2FpILn0UgWv%2FncLHFUhKU2vhf2Ul6AkfHYqL86jY1NXkkzv%2B5ywdxKm%2BwD5Soc3aP9fCj1cygXfOHM2nAv2Uwo9bo1nQs7Jw4S4lmy01x12dSAa7uJM%2FvskifLc%2FRl0Hzo36NR8EwoVC6N7czjXEYR5P7h255exXqJWP1ZZz0mJBTMORVXM9XCU%2FP0Fw1aJDXs4HjJmRGDczK4KpXBDL5YXc8sf269fVlAqODMnYp0hKmgIRzfAzWVEYzi4%2FybU8GS1CgQBqu7SsALgd9oI852bQaQzCC3ysIUheeh3oNM5UFaw55O%2Fs4%2FZOpSkpYZKqkACWPtA3VvFRKuj%2Fml083lWR1QqB8OBGMebC8JbGqQ1GZwqJS4HM3BKzKyTVvKg3FE8%2FoSvoj5ekn%2BzsctAkpmGsiPmkaG%2F5U7pb5G2swIQ5QvFaZgpUY1nPk57jw646DO1CaWgnYxtYXkmsaeOPs6m8dh4NDLdkQDXCuKhs6a6IwwQ3zjTjhC6q9Au0%2Bu8a4PQFjuzpg4Y8w%2F7SMyAY6pgE6RR%2Bx6CgR%2FruVOi8s3B7CEVf9MoQZplkAQfPZBWGBdusgzq42xQYqrIUFo%2Fh4lVIzevAeWEknnw%2F4oAajdbitA2kSSo4%2FpeGLx2jZap9MpI2irlwTaJXEP9ILK%2FdepRJWg8cxzlC%2BTERYgW%2FqNm9cwfcQNCX3lMyz1sEY%2FhFekxX8a42sXWHiZ1%2F6ZPTdE4RVwgLMeuT2AdOhBtq2fkBzXZT5j8dY&X-Amz-Signature=e654ed3dc9e4246e7548a286b928617bf9eee1c1deaaaca5a3318dafdc04bc0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

