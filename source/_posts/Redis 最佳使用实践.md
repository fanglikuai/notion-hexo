---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEHZGG%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T110053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQC9iyWSh5C212lYj0PUaJSyjbLdCcRepVyar0Lfj1g2nQIhAJFth0EKRce6fMsZ32omj3ndLATcNTolrm%2FwkuLgCkD5KogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwv4WDwhYkUUWb%2BSCkq3AN%2B%2Fg4feq%2Bp3xp35TeEbEXa0LqY0MGU00WrdEppFSUNAAJuT%2BSCJ6fj2dNG9HGyGTkfX8nHEssAOmfqDvPg5oJxQt0Q4yXNgZ3iYKYwc4rl9zPzjvsHAw%2B5x8j2EnBYtyHQWUhC3C0Q3phz6slxltAodgU7OW%2Bd2F2xFjZT%2BKeNyqkDE2EXE4t1CALje7ySP0oCT6muDb5F%2BJKIn3GzHitbPnwlveVKBB7Ot2O7dJL4vdgp3zZzY%2B%2FCPK9ztZ0axqNIaT5WU%2F%2BxxIg%2FRKmxuH2cMxyd8fWcuzB2vp8d53a7VID%2FBxuI4ZqznlLUd6WtiC%2BVIr%2FLr491e%2F3rSOs0bFEECdrEjn%2F%2FCBMD%2Bku0VmSrkAh7gTNrlmZkvv166CFgAV5Miz5pFuNAW6AxRljUesBhkezdJSt60hoJoxXdbhBI65QgPt74YB0LDInL2oV2gFFBveBkapHqJ639jcXoAqDg12Un%2BXdg0Kvs93hR%2FkZvdY6xiXqh1jlBtEbzxOArFc%2FNMifcmUaWFFKQ6FEHTOvcuvepyyZ5HYSQQPY5rtYWrTUEnEse5v6HJ6YXmgLwKJgdrx0x2wOdBiHypTITjXPoOXWJf2Cwk7IU%2BLGM9D61hAPKgfnp7QfE12CmqjCX0rTGBjqkAYQsYdR0SsH8ksnDJt70mHYCQnT9H8bLTc8gR8pVF9Nrm5uJ2pn5Hx59n5aLnwXWWo%2FgJQe2QWOXegDUt0iCQd8S4kaUbfsntcS2O2hwBfifalIGmfGRHUMG8hcHXGhmPpsqheL%2F5vWnL%2FSowg1C7Pksbvx5xuacUdE7qjv6Ik27P6Tg7%2FCBXaLAKBZG0g9LKLKcAi6ha9CMFPCfw%2BFfiRZvb9Z2&X-Amz-Signature=42f68d9d9ef05c982a351778b16523d81474120b7d86f28724111cb1de553ad5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

