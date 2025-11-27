---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WVPC7KLB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9zGmgE5LUUTy2pbWisTFJzEJ7HZD%2FbRNPkcBHjDLChQIgUEsAet5%2BIxtFBPBq%2FcwHhWCqRgWjHI%2FwO9KXFMD9D98qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNmBSSWj0a59XupbkCrcAwjTFvJJOwhB%2FRZ18i0Sl9Jc1j%2FaFY5o%2F9ifiS4eOwG5jVOIV2Xpc3LPwqEGTO28THWCaWq7e73TxN7MpgnhT6KVlH%2F3%2FgGjlRqfGUdAIWhVSB%2BBgCRvZJ4490KdXDtxhFAo8T2yLrx5AY9tqutRkySrmJxea6vwy%2BKU1MGCLIwez6zrVYQ8S2UGUPD%2FmwwP5hhXLDQEqQgL2bC6zOWgo3uu55H8D3N9sZMCns%2FMJGGqjP6vbLcVMBaiiARJE3JXv4rDw%2BaNF%2FBIm05W1gwSMKPzU2o1ch0%2FuvhS8J4l%2BwnAJZjssiQOyfP5WTFezYkmg7hmX4ZXXCUHhBlzCZI5L%2BA%2F4DZpfrfJTiVmAM0yGKIXABccgfHEVZ%2FIb2tZ%2Bvkf7yqHfneLtIQxIPp3zMaVKwscTZ6FamZxvseXXWNjrOhXHKOYrdA6fw5i8dtmoPor9MlrXIartULufrKkDqJz%2Bym5Nz%2BY3w7rs4FNvbje4b6VaDyxicmTRRU9z%2B9ALM0LoOLw71CiN%2BDv1c2qxjKlfk7oNnYK4jJpIsmaB8S5tE9lKsVj1w9Pi5FLjYjH8Hg%2FKJr5xU6wDzJazR4VqeM72XM4ddFcMkvAOXJe7OljXKHMJjloCd5X5BmHKcxbMPXEoskGOqUBfGymfTUBqc0JF%2FbpvQU8XixXex%2FQ9ytskO2vdDwJtnphYuN%2BwtR6scLDOQt%2F74GFcB%2Bj4wu5G4H9YOqpZebguzsoiNL5kZDZiTGPWLuVPmSbTQKypNjKNLtfspx0NFZuYfH5xL4m5DN5YzgS5AhKAVKqGKfky9MfO10ZvV1hwWlLl70MgiL5iZIkVN8B4EZkSRm4pOiQwOa%2BzcwCiCczrMktdL2L&X-Amz-Signature=e52ddda89f0e268bcaf3061e14d02406b9aaf660bba520e0e1acf5b87f4c2c17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

