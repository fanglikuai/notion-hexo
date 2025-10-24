---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UL5SHCLY%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T130104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCja4eIq3nnb%2Fe5Yowda0oHWqVU3g8XyLh9vusVjGj5qAIgPBrvM34BpoxYIRw3Qt%2BPqKEfstqkOPV8yJF34LECqtoq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDNbmttUBS7OkLyzYCSrcA7xtwzxbJuiGwj8OkEzYGZ8myhUaFYIYw6YR34EzU2uPinIJoetMXH%2BE31B%2Fa8qaUK%2FjyT%2FFUel2YVC8LsyoUpW1tqipLSmLE9MzCXjljYBB8uhLjGZJzaGTUcuWMmckiBEy67zrox8vCvZQ4OkGMgrDSQZ7nGGY5nwp4Q9ZoKAkIUfVkjUU4pkhcKmFEj%2BqmV6RFpYzJqclrewIRn3a5ZnfoCIw%2FMypdRtYj8Nt3cS9%2B%2FlrczkHaQa60yzhew8pCqn9782hqxdQyKmpXFqffPQYnN%2F8jgeOG2d9aUV6%2FwPJXPkm522FgFZJxlo2bZBGrdl12AdTp9wlWBljeZTiKxa3A%2Ft6GZh2M9zOCBLXNy1Pt6jDpLveptpA2tBxeNZyMYMXPOwgCfFmQXJZkbsAkbWtz0406BNHMQYl9b1SY2FsXvNbbstevlkMLhVXawhG2OGo%2FCS2Ofz5AcSt1m%2FVkT31nHm%2FKLLPLUYWcjc3xi7Pkq3%2B%2BCPd0HVn4VE2Sojm4AwBA9EDsdmT%2BokOKOrk0loLWwfQpTV44JpY7jhBuBMCruDwZq8%2BXJt0Nx%2Bby9vrYMga25eebC0m%2FDeKysUV31V7kdYMlaywFnpGCmM48dg%2B3ZWyu8XEjvttt7WtMP7m7ccGOqUBzAFVBId6dQ2sb%2F9zwvNYl0ZYUjAEeuNfQ0Qb%2BRGHPmoCgwEjSxeqV9MN2vr30ZGUzBsdnsr4zmxMkNOZHB2rYh%2B1eEaT9i85l68O9UsaW%2BzBt55in8AdJpTzuR36h4yo2ueWJARZcDisb6FegmFDCR6U4L5iRBrcoSUlEKZNBHSSCx0k7uL5a%2BHsAGjGFi1r3jqY4BdsJ3E04Y%2FcM2mdDpwrLQfV&X-Amz-Signature=5d716c9ba15fc9e7243d0567bf4b0250b9db546e54c244b666911be2b3927f7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

