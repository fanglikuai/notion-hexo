---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPDSA7JO%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJIMEYCIQD643%2B5fQQ%2FZoJpPXRUI9%2BQ69RFyD4iXCDecFBnwOgDOQIhALS5s%2B6GF7dx3KM%2BMNO2mT%2BuoRSCGwjOptLe%2F%2BJilEocKogECOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2B6lq76TANv%2Fkz0Moq3AP4e5L3woeMIlO7wVozVB4HMrgFbZyje3XHkpQqRrpXtMlK%2FbM8JiP1ACUuOFe%2BhqOu%2BhS78xSHA8vtj%2Fb3zEykNhWf4e8UN2opFD1z3jBLz6uXIDUZtnubWvjXQjKsZ%2Fqxm9vWFBydAcS7b5iO44OqSfg6oTmWQrBkp2i7yngLBQDEL7njfl0s96%2FLhkjb18rxgVa%2Bg42Yz17emrlx3Q9yKXU4wxrGMr8P7Qw2U%2F4Xj1vsCIlsjMaCKzhyhJPkvblMspyBWwCNq6hih%2B1VUbvsyICVjAyN1bTQPFkOgbFdS0ALnr%2FrbpRCErTIGNHd0BPPN9cNT%2BqaCLyUXp7Merc74GUFJitSi0EM%2BW9WC6mf%2Bq1%2BWfP4%2BERxlsB%2FqwnxqPfuVQeteHxjKi6dNvwg17lz9WfAKnXX5lCsfIRX4WEReXSpH%2BqIcSnNSNkDrINrOSwBKwSZY0dU3qC29lGJmgkaTq%2FMYjRiFTTUzYlI1VolBdow0i1IS%2B%2F%2FxTxbxlJ545Ikgf59QhiGgiuXaqyjTT%2FwsUNYehCLmlpXbnPC2ZmYPyS7QJsWUCut0gaYjpZpiByBlvkjQx6x0iIZdx83lVU1q60FXTe1dwB%2Fk3y5GAepirFLG9dETD3HPoF9kDDslIzIBjqkAVSjSXEH%2Bua3g7YvAbr2POoeJ6OOYk2SHhQqpkGoXrZBj7lMTsWTj%2FX3uOdl93U89G4VPxkEnUF04lMWeePFuSzmTeRG6r%2BKXS1lThT%2F2xhs1C%2BwTQQ7IGz5UzOu9HnLQwurKO3RdBP3jKXQhDuQ%2F9Xdg2EEEB7V3wONnnsQtGMjowGiiF7sXO0cAwIlQ857a290CnUL%2FJVMVrWCo9StfmgL%2BYOp&X-Amz-Signature=2e5fece23bd7e7f6619ef55ac7b71ae9c2197f6217b4a30bf5140564b170a73a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

