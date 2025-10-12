---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPR3YWJD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAURoBQpGw947Ch%2FDIoy%2FOWf6dCAm94Ntze4aIs5oHk9AiEAgKU0tsT0aO%2FUO5WeoEx6%2FwtZSfJLxgJGRvz3uSRvj5Yq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDMjZHQH49ORtRJG8VyrcA7cLEURwiHa2O%2FADBRAWVhyFqNPg25lBCKA5QPvQZX1Iw%2Fm4IAM%2BJWOu4ECekzBkMdx8hLyXGIzy2p3wmemWjO8dFTY%2FBtA3cI9%2BaA0rpsXtsTMegNUqzz4wXwQCWmwKNc%2FaqKuXsZ7FITJ%2BJpYlYn7z8Tn8HWUr4FfXZnLNsOE2jAb%2FSaSI08BIl7YOO7NXEEeJsmElkEbYzru%2FZkrpkSNDWR940jXDMje3ByyoMcrJ3ql8WUVgXrLBkq5WtMb6Ps%2FFMsyk7ghHGDC98GrBALRWkc1sS%2BELNIwzmRsjlaKWbQJmYfkkJJCkl0pkg3y1JoWs5Yb%2FEIa%2F25%2BQJ9ksS8QvmFdYaAp2jlqZvhc18cIflwJh4elXGBVjrOGyO8kVbdSgobS1sM6jKwRnJOYs%2BqLyXkPSO97Xh5VUGgY8IaxgEZJ7rhu8l%2BtMK5C878pUcOmWej%2Fnydt9PaiXuPUxoV78WUfq%2B3rf%2F6fmwJe4lQtGkq6hp9EOuW6UjVOYyrDMQkBDfPrpbnbnBVu02D4YfeBWzYCyiaTJj4W6MCUGYdpUA3Nj9Qg%2BJJQ5JXjJX818Ll%2FBNoNAViYM%2BNGxP33tszcwSv3IlJ2USWb3%2F%2FvKVeYc3WV6%2FLdXqX6O6sW1MP%2Fqr8cGOqUBFOrn3Z7G1jCdC3fE5NiAPUB5AK4JA8G398UEXVxNxlIe90uiimf36HBr%2BlY%2BxdvEmxtt%2Bm02TGoa2wB8IlQYId0fOQXOnx%2F7etV%2Bld9uWnfKONcsnSC%2BkKU4q98tzT9eOPme2dCDB1LCCA9zlMHpk7wXxbjyiyX4B0eOtWg5aYqujdGA0%2FDFBUm4%2F1f8dNoufDoQ%2FKTDaMsfDASYb1yedNgHKwiH&X-Amz-Signature=8ee26a8cfdeffff19d24326f99f62e01be3704b03d3087a35bf35268ada4f776&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

