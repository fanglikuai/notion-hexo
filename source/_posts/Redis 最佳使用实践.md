---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SE2B2RTH%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGUh7JoE4eMw5QJom2HX%2FOfZwBmd05tgKacLbBgYG1RHAiEA%2FqoBXT5%2B2C%2BqZfWLkmmKzuBSaZKggDEW3X17sEHcVloq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDAsSCFtQsyImHSKCSCrcA0J81Y9LM2Mb01AjmR6JIz0b7fLeRhxy0NGKi9VuytwRBQ%2F1NqBbyKkjVNiLgdsimgciCy4arxrjHfuXuqzSNwJ0sA%2B4LVsy18AFUZLogaUF1Uszz%2FAf3yPgop%2FjNnxjlmfV9nQovTW3dWEx9RI8tB%2F1Lp6Kvr%2BJPMh7KpDmMQgagAK9e6oP8liuI%2B1dxia%2BJKlFF%2BTCWlZQQQSHBxGoqpVfq3IfRVhi2tSF%2BscS%2F1JJRMacSrEQrpZ%2FzJjQDvFCOkdGzD5GzQBMYuSw3fCGGR5ty8lBwH%2BR3GOioRqdEIzfiu%2FVxtWDRwXnZjKB1foDXEMkHMlIbZun72xS0iYyp8GRMVc%2FWQZOnNVDsMau%2BkcgmbbXo3ltcmjrqqhtYIjQc7L9Fq9tZ5Ew4cwvqGxuUF6S8dbQ0E7q9ONSa3kM0oPvlEXY6eW0ABLZTlZC%2FIaNuO9ofUJjOtFn2F3fOr0CTFPha4%2BoInUuw%2FZ2oKJywsWZDNwqNviw3a3BQJkwafUHwIbDkhnFAqa8cfKLH%2FZPCyCAIjTYHZ3e4NXgwuF4HV8Hz%2FKL%2BwgnvOgFP7Zmh8aZnQ34FgCYrOAq7kmLJiZ6Lh4Ab%2BF6B2Oq0qSUg75qM5OabwK4H4k90YMbjeyWMM%2F50sgGOqUBuvMFmP%2F1PPWaoAqOS2BA1ZdzH1Hk2Kqg5viQDw108rSBw2XQFQSRPtsU1ekSbC2O5hNsJKAjIF7Kh7%2F%2BorJmdJsUMrN8o0G6zI2qiGhoCReJQ%2B0Kt2Xxs5xNmsd8QXKHx2Wct9N4w2Rk89hGttb3u2dNDFKWTQo77ZROSr5D%2BPwsy7nqMz9zkeiiZV7HfPuBMK53QxgS7egCEZkiJcPNY5L1fGQ7&X-Amz-Signature=970c14ffd3a8e144378d81f7b46ef625c13a610a32062cae7d94a1fa3ba88f0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

