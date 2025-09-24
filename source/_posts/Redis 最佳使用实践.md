---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QER3S5TG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5Vlf3Nv7pyOcev%2BvCgURIxy%2B%2B%2B0%2FfJfC3JZDr0t2alwIhALg9MRld48rDQJgbpWx7Udni68tFIAQi8Qmbc%2Fa8HpYjKv8DCFwQABoMNjM3NDIzMTgzODA1IgwC5K5TATTo5tZhXFMq3APmZN5iEl5zmJnOTqG377mM3Efd7KfgyrdQbLzXA3EemwnML6WBwhpoeXHRs%2FyBZQ0ATzkiaj5A682%2BcGDutsxswcsTYxN4EOf4t%2BzqGVtZeKTu2dgUqR8gVoa7OTvvRexgXdy6VkJIV1NAERXGWU9mP1dv8herVCf15VN5B6Aouzks%2Fbrbh8Z2GKzBaUbHgci%2BkTHAxkOHgFDNxkHQ7EWqnxuKCppmLF1YVCMD3miIdvzJtBIO3VBIlUSrRQb%2BNCfqqk9KVaXhFKuI27vHdOKBkLu5AgEU1LUSfp2eH6658AnLOKW%2BEmozx5jweU6PiFxdQPTEIsH%2B91ttF9CmEIrPtoEWSnTp1luX7xKBNDjWcBoxCy64Tur3oN9mWN0YAKSX97gVWmIAeKRya%2FovFTGV831SlRgr3xVMrXya9YOPs8S1zMxpP6G2ZaiU32gU9ekECuoLNJL43wmvzP6fev8BUfZQWGUVYnKT%2FTMoBM%2FLhrHnemBWevi%2BsVEEqvDTG0TMNax9fO3hmJcGKfcwzt1Y71hpCuEP19s%2FpfTHAvBXdtWa%2BP51j%2Bd%2FPcaL0WsVBB6%2FBNhjZ22VkZvrozOc6unM77HM3byugmEcASTkflewAl836Sn36XHxHO1VDjDflc%2FGBjqkAUegd7k2cuEtxyiWppBaK0agrZikyryC2LUhzS%2BVpJf5X%2FyEEDQIqpz8k9T5zKL%2FxwZ9B17sxGtEjFjeno1Qw7Ni9Kchd5AhATdFf1Z0fbrkGT7iFe8ca73t%2FR32map9b%2B%2BfeNbm%2FPXD4qpswyPl1GcOMfOo5dBHkU%2FNOKmLQmduC%2FquxybnSWiSApG54ASfqQrkJMDQpjMut724pU547lolxkfN&X-Amz-Signature=2c7706bcaab037932086508cade6a5b6920d23a8fa58c7721e551c14108f8f79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

