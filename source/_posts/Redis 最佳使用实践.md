---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5ST6Y3%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZqO%2FjfmeP2YCYtbLbEqDmFV7XW5PoS%2FTzsHrxarF4wwIhAKGXiaX%2BVQrOcLe%2FBa0pKfpxxka4kxpVEelF2I5id0mFKv8DCFsQABoMNjM3NDIzMTgzODA1IgwjG%2BlEbkdj8079Jxoq3AMBDBbj9Z9IwbMew%2BhSnVJFnws5Mii6lFGpPA155dWdXoZ7z4JTvYf9mQmAIpZbwldA2vJg%2FWgE%2FTD9lmbol71YqVha351%2BPz%2BFhP3CwxbigrfWtgoIkQ1z4qgWA11Psim%2BnUpBhdttSPVcd60Ts7kZiDKzaY2WdlM9XoGhRK8INCW14JQtifyhigWI0MzUrZ%2Bk1Px2GAAb4s6THVviLk0ZeDzyq%2FM%2F4ATagskzl6KuLi%2BZWTV%2F2KnvIbKHK9w%2BcGZouPv%2BFw9lH9k8hc88GMUFusIegSmuLmfEnQkxdliXhYpecjY9ok3UNGjf0I03%2FnMRjTbYdchRLTorWxvmxnBFfAIWnoVgXMyq0eGO41uHp3tdgsjhnwHFQm%2B36Of73HDyNBdNg0njGYKbVI3uCon1x8rkIsn7zeK%2Fca5y1Uys2mo%2FK0Igd%2FM6xPXPzUw0Z%2B1i%2FKoypxnZeIxsfbOTC5HH8I2ogQbr4D3NDjjpNTRkPtCZ1UfOe3rBXYYQRGoy7QjdifwksnUc%2BmdbXmJ8jOmN%2BQcBvhqrJI32ym2AQPXu5KjegaNyTdLooubHgeFkiaBLWvktLQdfXcUq8V1W5KRzPJv7%2FZ9tdSRQ58%2FWlG%2Fs2l8rYer55QXuR7uvqjCVlNrIBjqkARfMrphGr%2F%2BCeYy409vuR%2BNFx41p5jwL6vX1pFZ3sNMEYe6%2F%2FuHnoEXYeDuOwUAXcogaQ2fX1XwCaU8xrbzT2bZ6LvhNYhxXHXL%2Bt4KSGRFp4ZaWfxfEJbgcGjX9MSrMkUAj%2FkOSDGCZRlSuRpNFOuLCEUHCGuH46Y%2FQ8J3tzSpGyAyA2suqdYnsRAHwtAlCydv56AsFqgjQeEKsZoU%2B5gMfIrh9&X-Amz-Signature=c480af51b543967156e19f7ed44b88c2e701c69d1838b3b7aedf4b20032bb639&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

