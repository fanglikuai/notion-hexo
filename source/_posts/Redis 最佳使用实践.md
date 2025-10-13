---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UKRRCH33%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDLq6uiaffAbGph6V3zmxj1brYn8zf%2FKDxhX%2BDDM7BsWAiBVO34inyldSd2Gw3gRxJjw%2FxGGpXgyzrbqqXc9w80Tpyr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMltOq9ejjf7iJJpDkKtwDAMw50kBxjh38PBP05a%2BRe%2F1zrhBquQdfPg7ibNQ5AcoWVgwbmtLLwnLTt2sywAFO84FcNl9IPtK19fwB5wAqH7Sq4bjA4NCyRS%2FT0OqxC%2BEbmGi9VGZfXZ4qloPsmKWxXn%2FV0sX%2FMboZLwB%2BUGRTaIVNcqPAcNfWWwLOe455CafQaekIBeO6yK589PehLoQqrm9kcc4bfb3cvcUaxd5NW8rRbcDn%2Bo3NBaMtyyy75lCZUOEEjshU7H4OQRFRqODJbXSlcmJqUlK%2B78wx8x%2BlWq%2B9F2OmnckB8kK695UiY%2Fq%2BzII76kOJQAgJQdIHJnAaGqNLUJLdZnyw4SGYZAXk%2FFgwTvCOW6FfpQbGp460aVfDR7XImPHh4KEHUABsAbMC%2BJWOz%2FGF5KHecKXxIMxhYzqIRc1tAEHJ%2FUTzkLniq9Y1vX38a5LOxZ1buqt8oUVLOoDbliisb9Snh4pqCFIBGX%2BPED555EzPkB2%2BUcP%2BGZXNzYXLcZ%2FVYlyItcVZ5vp26n5tT%2B5B83617%2Fnljot3cp9LawmNChviXqt1JKboqRUHNmFp6aOH3foHmQAvrFFuNrxLtdUNgz7XPlW3t1C0C6oP7Chss%2FI68pR3x%2F2U9Luyl2kIiAs774fKwjgwgtWxxwY6pgEO0lud%2BqHBpKSVP%2FBRzFnzSPD7YOMn7cK42M4tEfbJkoRQGNKIclGb6JQ6q5vwHyHCN5X%2FbZdvkqC4bKmM5IyYFsPn0NZryTnX6j1ulhr4MGCSQHfODRwblEitWO5xiby%2BADgo9XRmOiDp9cNrz0azDbkU9sZyhYF5IMxBBKvYknKzVGfi6V5kkgEdsDFmZda8DRX4BP6qUHdxBTOm2ATCwTOXlE1I&X-Amz-Signature=297698d86cc4b34104c2f4887fa00178b025d349ce161eda2c2a3898cb384004&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

