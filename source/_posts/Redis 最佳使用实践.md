---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYLRZZ2%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T110058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH%2F1TAQL59HzyGUYMRUlheqHEkCfM5SCYImBbhQd2KP6AiEA%2FaQtyoApy479SN0sjJcVH8kBOaxGAcD1%2BtKcwZqOPSIq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA4QwTdZybk7A12aVyrcA3TVeWUYa1OBDb%2FHb3y8c9DGqmOC8f0i3zisQDBZz%2FQ4dhBGd3Phmi%2ByLJXIAZBcfBQQxt0TopLs%2FH2Jvw%2Fnb6pO9HotKXjczTPFQuYFghEKjSsFVpN5xVlCrvWsTb%2Bu4dmVJMvKAWHqVyT8XPYoKWnqb2D%2Fc69ZEw3z7ffDBqpb8zP8xj5ynbUbNb25zb86UT4jHzswAOVlmKgyfQ0LTML5xEA3c3vVXVpR2E8HmNA8s92vqnxoqU%2BKB4J9e7%2BunQ3bGGjnXy3eZamp5SpDp%2BX%2FsRREpMEaEziMnLc%2BV%2BDLFxatbMIjR17gwtG%2BK1k9XgNlN%2FxpYGy3%2BFfCMl4qTBsQPzvKgdc8S1yyBw2w07sHYoLKCLny16mIDTUO2yJ8vL1sJykRJ3X6cS1kJ64C5zoHMitg4KzzZCjcJ2UlOqHvhaUD1NpHcFLWnBzqxvkCO6KbWdXa%2Bkg1dZUSEgiUWqR%2FnZ0wgZia%2FLdnAGDPZMXSdn4HTRJcuTii2jpFvG3IABhOeJwVEoVpqWxdqiNYV6Nn%2F3c1ojlagg2p%2BLBGR2tCnPdVEQ6tlsATqkhcVCO9eibTgGts2hExztnmQslkJXR5Zr6zg9FZnqm0pBInVQ5%2B%2By%2F1zOBXjW2kdrTKMI%2FsrccGOqUBkCMbpyshdUEXhsxH6OOngsJP0u97774q8eFCSBPvrNTBmVGYdGJ%2FZmihWXOBi4tVPMunDADDodBJLTTxIK6CLv2GsvXIE3VCA0dnOM%2BtalyGhlIkVUMl7%2BvFWeaPNLgVoR9P1fPSOyd2H9uR33kmZDr51lfiKrxIm%2F%2F8znsbG2lPWd4mlaX4O2eMnPvInbBjPriHSYL3MhKYCNQancKwIKcqqFiq&X-Amz-Signature=d35d92aec16815b2256690e0f14cf26420a8d3b61aaae6cee6bba9b376eae69d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

