---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFM3UT7W%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1%2FvVs0C7yYyipem1DLej9dbuBvzT9cCHmDdvbpmMoywIhAKFQWtNEdMrH%2BpboFM99MO%2FxmaYXP0VBHtmZjnUiFJ5TKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwmMCxTK6d%2B8Cwbqgsq3AN1MV5xJxdx2JEi6VEjzSQ7Ng1usA0tHHUPQ%2B6RkSHWtgxwrmrXwpvMEbFLfgLbLV83BghyjJ2xgGstXH1PCpNN53cw6Nom9AqIqZHfZAoolorMbsUON4PtgTnf7duz5ZsedY%2FCkU10ZNbD4Q5%2Bp0kxJMUDRUL%2BLbTr9ViMCTo4PI1nYIcCdam7o7yhSI6SapgrJxZjiBHPBGPydsj9QLoq9QrJQMdzPx0e89nu%2B67m7DM4PyQXiv3FnkwpnhVTEgKDhkARD5S5K6mKmuTIkLKivt3gx8Z%2FTO29dXmagsVdXUoP2XhjDtNkysZHLXYv5E7p1sd9kavDFPNj2z%2BqBFyHpCkNq7IYAqqH43osYixOI0OlsamM1bzc%2BABpERAWdHNgcdKrGJ934KjjM6h3QHZ8mRarzBHU4HelVUAev8K8dnRWtABCsKIW39D9IkxbjZp8eSRyWc7TNM1a%2FNNR7iZHywgPxpyE5Mz2ZY%2F1GlIFE9gsZvVVj%2FjUZV7%2F3u5ybfnusoe7OQsdQqZswEH6u42O2pR0iv1twrO44Z1ELkVkVzZVN7RuZ5Y64gPMCKY%2Fu0KuxLs%2BPUuCRpvhN%2F%2BnPCVLuZZnKBs5sYHgYlsG8LsqykJQTVn%2BPVyLrk%2BuszCQ7unIBjqkAepid3dT60krYzjDeBqJvQO77Wi%2BVrbRcabUs6BPlZLODV5RqaPVvqVSDfoaM%2FWm29U%2FRzkAhXJAzdH7ctzpXL3Uu0NOySjhi%2F0L34triX5w0k0mMqn86TaIrcEUwkZoG2OqWAn9s9aFIBdtFVQ1mlpV9WZOqpIwEwZXc9k7fBINl7Vl0SmOWTSE7i%2Fb2FG7aL5iWthsKpVYw4PK5mqn6XBHmlRo&X-Amz-Signature=7ebee50eb56582cec12e284a6f5d807e668bc5ed8574877c6d97fa1542a3124c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

