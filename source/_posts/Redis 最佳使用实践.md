---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IA6XS5B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDl9OVmrV8aUdN%2FKCQuqBfUA4M%2FpSqRTg9TN5VezADgcAIgHKJFiTP%2B%2FtctxoZmx30%2BWcQTHBzbvHwf3q%2FryzkOtdAq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDNWUmBeESny1evIMtyrcA8MKxxd5sff0XZObdzt3sbzh16dhTbYMWrgV7sRJul%2F0YxVS5SlnghWWOY43rkEfoG%2FODPKrSUemjlC91BQGGwJ3gzvZVTi6df2alboc%2BqslE8mjz3jCh7rd83VVYGdvxzA8OpwVXPFeqa%2B57YuFMMf4KWyDlY3OwR%2BdQrpzLi3yaIqwEGQAb%2BsiwcPIUn9F4MJcDhJojh1d4lV8mf9Op%2FLjarl7ci6Q1%2FNlmkkF%2FLG4Pk57dL0JpI0n%2B0bb34jzxt1awkP5z3462naxjIsO%2BkGuPHg25dgvkKJfz6ZZz%2FOG5zK8h%2FlKXGhazSN3NPmtmMKOq%2FRzwTzBpbaqXLsoA8QVo0NeQl0PG%2BlwQoVO3JHQBwyy8aZk32koKzNI%2FphCjEA0U9aYqVasgB2n5X0mjuCHDOuXqoxNJRte5W4fr3aDpqov5XdO5uGV7m53GQwHL8QK6spxLXNxYHAfppX2MrtHGy49haB%2Flq908WvZfI08TwU8NrqvH7xPTXsz3g87LRYKtCiVIIylrP9mtKxsMzgdXvwgjOK2m2OT2EkyW7UkA0G5TmXF7u7JF9ihirm0%2Be4HzlI%2BvfvySJTQEGCAlN%2Fi8ieYBE70zL0Q7srdPFqwWdOXHt%2FkNkE6olp3MMfzuMcGOqUBVFMxPICSyyMzTdrWiHFTIV3zWhDIsWBZbe1qjn%2B5FTwivhFtWmE%2BlAtq6QpBri1nGftousA%2B9gsJSic49l7gGcdBpNs%2BaI7zPGlhLNzjGTNigMInQcIydfkzjQ2wZwCFHE1QoonSIWo4NouuIhEXnszP%2Bx4iPJ7FHF3Op9fuHuoGFuCbPZrks06wG7XH5lGrgpojmXD4EyAcKbxIaEZQjzr8zq%2F5&X-Amz-Signature=69d6740ff5c26ecd5126e656f5dc72c896d087c1cd3e62a92e50954a6a5c66ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

