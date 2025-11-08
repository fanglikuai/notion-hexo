---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVGPEFQB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T010128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDWSnm50rBvKvkemt0uoWuGflr0F%2BOzxitSyTFP4J%2F6ZQIhAOtJGrHxcHXeDcTdlT%2BaiQBUJekYa0QMy8rv1IEfF5AkKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDgnWsFEaOEPgXK4q3APWhEtIF4r%2B97w7kwjegdneIRr62MULLwGfwIta4wxf8iLyToeNTqqOKqbxZJdJJEwfZUyVXcqdg3rtmodIILBt7JezOf3Ak2hmL2p852p76oTVlIJsQO58%2BrbO0ROJfiO6bBK9NoEo%2BeDJGYKv5CHNPNpEVN7gmgQ90pQntYSccJ7YQfjExwq1jmJkwjJaxaS4QXnnATXJyg2HTi66dXJf6xOtm5%2BxHVU5nNpkkX00RmJHus4KTzfPoxS6l15wt3Cvp%2BhNrm2Xhs6BzjRT%2Fkk%2FxCDH669qfTAqN91jqEdB%2FcxgFHtO31UMiOiKZQlmka1oVlp24HUzPDHIE4FghvRnN8I9myyHwqNwSrm%2BwIdy5%2FYhUsZZzMl5%2BiRbuoBtPPHPH%2F9ML%2F1B6Q16JdCq4mz2XdiqECDjXXMVxaucIR32XS6KriDsSfHg2ZO6%2BtmyKrG4UZfHdpCLNhH53jCWb53Db4SxN%2F6whFo1BbRlAGE3Y44wewIPMTWcflE8cOCpyk4Btru0V8aujOn1nhZ%2B0E0l%2Fs2A3cXCfTmDwGjVgXE6Pv29wRDZZ1GO9lC%2F%2B6fGCVeJQvLVnawZAanc9ZR4lYgaU43d0TUYmeqbPrdXJd1uhFpuuq3Q3hg2ldFWTzCcm7rIBjqkAR%2FSpXWt974c6xS%2F2eTVy9LWmlkPwO5Z%2Fx6%2BBnKXPJjgMLffl3TW1epdjvzjlW2JSCly23aLXst3aRP1qEvq%2BHIdOCx%2BDYOAAPIP5DLWU5RrhM3RtNMgnebnu35FKy4cQbM%2BFAGS86RE3%2FzeLcO1qmJ00oFrLLDTCohKDIPToTUtnWYdVbpGj9NHJLnjgi5fqT6VIruDKPA2jWUJa1tEXtSNySv7&X-Amz-Signature=0076f4f12bb1248a6e8042941b77070b98ca506f3518244b3ae0e507fe0b5524&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

