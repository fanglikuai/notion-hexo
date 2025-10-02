---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVT3CQP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZRCS2QcCibxM%2FM8TckEXgkfis%2BGOZrBTAYlWHuAiypgIgZ%2FYja4CiMTzzTBD1ncckbXairHo3ETCa8uqKI8nu%2FWUq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDI4t%2B%2BUaQQ3ZrtuzFCrcA0oqY56ZpJFzjWtQqdtgxEuCwSXEvoiJLZqlh%2BUqCe4V84CcNjoCPKI%2FYvkqtO5J6EaXiMJGFEB%2B6gCSdMkw90djCbSVlHS%2BdC4TnDUt5FYDpGhWNxvRwx9FHyraLmNdnquab1yH4LbOBxuFRqoW%2FjgGFW6LCJzbUI%2FYCPHfmgrmthTK%2BQhuRM3vHA0m6WX1uwH%2FpGoZQ5%2BI4tTOGDc40d%2FNIkcXTay%2Bx5MOYfhbBSSMK2OfQcBrJrVwqydibVKse6LBh9NYzj50mzscS0yuOnVU%2F%2F1MD8hNOLDV6%2BbhU7UuxqIu0UTQeW09Ji6z3ctV34VA8Bw6BWffEsJsGFoUqQhW%2FQT13wU23oEN6vu%2FF8EP26YK66a4G03Oz2vup7aHcpVNkSQvWHgkFaLWMYTG%2FJuzJ4sD8C1ZYlwwTOAtwGu3mALHidAW546zR1cDPidNI7AxyzDJCuIF%2BKBdCuaZXqDky8q%2BrjzDdQB7sFnzKPPhGJdUVZI20gYFdu7zlLfoprjY6%2B1MV47YW3fKfix4HV3RC7EwJRj93RPMGRSXH1%2B1T5G%2F3rhB0p7QiF48coYyEzhnIa3AMIN337vCcydyz9DbdQhEy9FugQCktqSEk6gRDis8gyieKBKeTjH%2FMMnz%2BsYGOqUBvvnzByWReODJ%2FV6PsDv6VP%2FnrLuVp2Z4VCRq%2Fq1sEVeq%2FBFkqH6lQpUtdrrweA%2FsF%2FHYxIynn4NYamcCAOm%2BrNeWN%2FF4zHDCc%2F3383DHUm%2F%2B2M%2BglEQ5oty9E8KAMfxQHgB6by0XiZIhjWd3C5Z%2BYd%2FQL2pUyZMNg%2B%2B%2FvRsZ%2FFRkBy2ObqwnHkE9uC2w5Sp3AFCZUNUQFsbbwgl7KkVVS1aiI%2FHZ&X-Amz-Signature=4a5fa78cc7ac9e52e48950c1d81d247db35f2b4a5bbd7cfebdf525e733ba229d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

