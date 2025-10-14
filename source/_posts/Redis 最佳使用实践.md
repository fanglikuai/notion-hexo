---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSYXVICJ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDopm1Zh6gisFfXB7xztnXv%2BFnInBbAjVWdMOOVMiEDFQIhAOto0oYp56MkJfuiZTHUX8R8%2Fer%2FoZLNzSwSYi1ThixjKv8DCFQQABoMNjM3NDIzMTgzODA1IgwwV81IG%2FklNoMfqPEq3AObwWQ%2B1Zoom%2FODRIPDO4f0eRPwBAbf4FTnLZPeBjCVbjheLz728tEsWL9NBTqhqpLrS%2BuJ3AQPZOTfy4Wl0K0VJzm7RoP%2F5LEhabanP4yyjdIpKcIRK2LWxqaUmmx7S6aTGsHp5od%2FbAMMI9lIw4S2GzKAVHpJb5OpEHrNfZczeIkQqz6PzEHAgav0js0ZyMkRBSxeb%2BhXxIg70PpJ4VI4X9JVu3tBEE2hSplbFrsZl4maV6ilMHg9hrVjSIAQS6lA69dqy67U6yuTXRbJJPYq8OCxZApOKNfqxjMEm4MLdxGKv06Yk2ntpZPBIT4hNJj%2Bau9qzrjJC9v8MpetMLYBbJxYmU1GfrcH5FTFg0t59Amjm8aUEzuwq5F90ZUlCBFEx56Im8qYDx%2BrXXoJp5Rx8AIIEoyzIbw4olYXU%2BXaQJJOQnmrvSnfJ7aAEFZ0HszKL8rr9BN0%2FrlN4JkeVrCQ0O2fidVwqpawZkCSQ2PhnqIzeMmDmlCGqEpMHmIGUK8u%2BgmLckcsopXinYwEaZmQl4FgM4SY8KkGPHFwJ4tBBz5B7ROHUeDiXSIKRk7PPhZTNASAEXZMJu%2BAaWK%2FKPEW7z%2BOcFJli%2BEIB9xJkHwfJOLqPcwe%2ByJMpabx%2FDDb%2BbbHBjqkAZVf9qy1kNRLg2reuJiy3ywTNYtDC8VEjy8nj45fVyWIkTJwJI%2BXxqKWZiesH0woI1orsgNWU%2Fd4JpcMUcMzfFL26dTmvff0983ARPPIaiqUgIQppaeXD%2FgD8pQElClsoxoutV2YfF0ZxUN7cZ6ok6yG3r6KlhLlsaWPbJn%2FM4REdgwIgV5akvmBODFow6iejIdakFUywTNMl2tc%2FvbvwZvhTDp4&X-Amz-Signature=7380f86d788fb4d446c4926195df49f6245e083ef5e61e4d15d403529fd8ceba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

