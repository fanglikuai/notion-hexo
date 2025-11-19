---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI5VXCNC%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQCgN7%2FQjDJLRC8N3OjtWwI%2F8FYqiCfJoJCAOYiIXaQlSQIgH6Hr3isyak2KBo89lIl8dFjTPvux0%2BPzgBFr2t94pkoqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYcOLBVYj9Gf2yn9ircA2dZm90UrS5wIUx5KAjZ9pU%2BeDllQA3XA%2BUpRq7v8XdpZBJgd7fFYR0PBeiW1CYdRmBQjo4ik%2FJw5zz0%2BGNhCHcBC2D%2FfV2Oce%2BMxFL%2BJnFW8pOHHYzCj7Cw3nR9DPu7mCW6uCahvS7i4oknzRDVFlIn8J88A5nbqS94LW2A7k52jkpBgjdoEjhYizTNOU5LBxsp4Eg3QoOqEYC67kY4Sz4l69hki4DJro5IaWo%2F8PNVQCqiPllmqkhugt90zCzQAoki%2B8xbJNAHouTWn3gO8E8YbAzUbkVTBICCmSOFazz33C985Wpm9v%2BEGdzW%2BbMFAutf5y3dcLzAdJ%2FT3MFFEqHlHwQOjX9B%2F6h%2FS88blvMkrEu8gr2zgLcN4BYsu%2FVnIa1eH0IDjyigAcPanpNqDS1m5%2FNCuTYrULTKBUQsHjQlKD4XFaRgV%2Bo%2B%2FDB3NTtCXdR9BTFUQTSnH5c30Hn3GZT80m8gMzznVzApXNoFqPDuD5cNNCSQSLRHpYnv1GFXQxFqO%2FEEFByWRPJfGp1Ma2jRYkoo1jINXR957P78wqr24TW6UEHXGUnAFw1Ia5alo218g54UD3ykGtpPd1EeOD9dx%2B6wDwGEM%2FKP5MsJJrWECKrEu%2BHGqEjwcZaHMO%2F5%2BMgGOqUBT2gPTGHDZfukkuOd2WzzUC0todX8hCJClVFly%2BRFxHC%2BNTFymQT3BelahBKI4BWowqWSJab0ths%2Fj68T48F9xC7xPuxhj%2Bqx%2BFMGvSLQAN1ponUPSvpjUhKJfSt7UEmNhOjT7xJu6MIm5exSH2Kkac8gnLRYIXpUDE3%2BaxnjkxtlllzdS97CDUNovQbmdMpm8%2B%2B1cy6Wh5NUNJuvMYxibayJz5BU&X-Amz-Signature=80176d812bb7703e7e66d6774389cd68bcdbae9da851c9873712ec6741f60be9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

