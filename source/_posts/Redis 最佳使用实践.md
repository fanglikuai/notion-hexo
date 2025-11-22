---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XU7R5ZAU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHJqH1Up1qYDgFjdxJ%2BU%2BLQNvbMTz3yRyTuSaoQHPYy7AiEAvlslvcuydaRhipj%2BPuKHSmpse4v0w3KC1%2F7QX2QLsMIq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDPH%2BaC1xdGFJvAnk7SrcA3uYcftsIW%2B%2BintdC9KvSQ1E%2Bj3hNBlOP%2FUHeIgQvTYmeKTMkjFJ5RwfAXGi79Ve87w4KVCzZBxyEjqETLlTQAur%2FJ%2BAid3NR%2Bp9V%2BAnHQnnUnN36Kt2mbPJzxpN1rpGTjibq%2B7JsZiPJyt4ErCm%2BbeXVl7Gvwqa7f1O%2BpLs6AkfVrNKeA9uuqTmRALCuycpB08uevQGUDE4y3hhNwCfj6uWxHckZvVzvS%2BDPBtVbG5gGl89%2FISdpBGuldOHnDfSG9EXg7FAPepaPb830I9f70ZIjif7G9kzsRSOACSVOsgPolGFNprLI4rcFWbtvz94QTBxyJpNHYoHHwn78PMrJj4tcJlCbhhXuhtwHR5d6WlYVaTcwPIAxKqiV1jsQilJhkVXwjf1kW05DzDAyDQ85t9D2ZBzGTBtyEVulz3lp%2FUx5sYIbV2jyN5LiIZv1Ke06aY2Wo%2F1YrcZBecIX9r4TcBXxDKVEKM8lvCVN7MILw53ND8oa1kRpufz81qeap2lNVeStvYEETJOpHDpp4JLC7brJflpfV%2BSzl5mxiJvdUImroxWo%2BKZNBNyt4iV7V7uYHf1MLA8gH22xTmNDUlsjTvtVGFuZczU8GzqwIfszQGL1h4uRKdgo2%2B9GzumMNPKhMkGOqUBsmw%2FtsXFhE198NZVbwvN7rrEMsDfwDBVvyfARUMKeVkJcpw9TKGJzCFxLFunkaaf7M9TqxLpzE9QiA%2BEBADY4SSn5lXx%2FMajmgmlP61cKHwGKYJYdfsk3T3YmA0XBmsgrCzVNwuI0vJHN8PC2bOp1JV3Nj%2BNoxhZxXcjFiNeiwNPYSFjgJUgsF47l%2BU5jhVH5zv8cd%2FvZESEu8EuD9vZGn%2BuI8xU&X-Amz-Signature=7dc19f398c054731b307f9d58d97c4bbe0e2ea9d8c536261fbb82e53ee587568&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

