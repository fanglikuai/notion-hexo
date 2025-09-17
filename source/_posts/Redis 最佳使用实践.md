---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTJA3NCI%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIQCVn6JNTgZIvEsCWlL%2FgnfMKo4fPMm3y9qtK35XZc9K8gIgNZvTpVzpNygTCdWo%2FpujB7Nkup0hP7ADRq0OlfCN7GkqiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJQOlr6UQEvC5ZCbnCrcA6KOj920eG5tyX91TMA3TZuZ5k%2BogiWZtWh45evXPcBVCEKUrR%2F33Rto11P%2BFUZKBXhZG6pwUpaZ%2BPLNIMNSKj4abhZ5iYp4wMuOqOW02BCcA0uODnBugILZgdJ%2FHi3f3z9pCbs7X70fKgcHcLOK%2F5FKqHfJb1wiA4blaAChIR%2BJ%2BngvsuHAJpafwBjUOTy63lU66%2FizoOKtK4EWvCdgtOZ0ywkT%2BG3w5DHckuX8SN1PT0uyLiSNSNnKOV%2B7i%2FyLhtizJM1rJscsWJaY0CF8gT6cdEFgE12PEMsCWUnK2PyeQmgmG8XvrcYF5E0J8x5dh54NZoUKlLOWauwMsGMzeS3xXYyN%2B0NINMJYJruPC%2B01jsWBGydTMlAYhX6DyvrG13b%2Bvq8WIs0yiO%2B7YxsLY%2BQJPpTjNAEvtjw91FXkhZowmRBDC2pGMTj3ThWz%2FivuSTJd21dIOR875%2B4dZ02biLcWF5YYVENS9iinQdLU0mHQcSJ2dzfLAMuMmoz4dQwQ3mAjxK%2FhVOOqamNBVC7jwTBzgx1S5hsiGj%2FWJwxaYYEHX6FkhkY0MR1bV4jVUlgnK0%2FXeE2HW1gdhttIDLhbnhp9z0xGZnl2tTKfO7SPnYP0ii5bxaK9o5AYQAt1MKv4qsYGOqUBRLSfRbsx9q4fm6M26DSWDuNRsGt2tL5VbgthlksXH%2BunZUu5YHAwxHAmlHgjokgxM1R%2BTH5ehC7su87HXDM74%2ByT9tfzaDQARZvck9hKzgpkwD%2Bg74aW9sH%2BbN7xH1i4uTjyttNNHq8qed4m9fU%2FYhPrKCoxujlRCyCrB4%2BNUxUqTsOi2A%2FzjvCEGX9TOTc6oB2Mx19xKnVYvKMmyUOAhOLEQJzv&X-Amz-Signature=38a452e918ad111339ff5cdd602273181a51fd25a908e90545cad9568ffef739&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

