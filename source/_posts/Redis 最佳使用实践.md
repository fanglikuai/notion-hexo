---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OH5X4KM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T210054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICSOnr8dchXqynrdLKafOZYh%2FQiHadANCuYTFaWR8iLKAiEA%2BTwAQb9V8A%2FvR93KdAlLr6LvJhjxi4PqVYf5iZWbuSYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDIJMMQu%2Bsqf6KiNpASrcA0uUNFBGgOVR28KyXKz5WHscKbp2%2B8kWzPXrRZtQuo%2BHPdsKDsgdPH1vnYQ7vJye8fNaMFh%2BcSegwOcr8Q7G%2FL7Kry3HN8LFfzBsS%2FKYPN0jxmV6iAxEt1sJfl%2FCG3hcxnWhqMpIY1ANmCmjXUUtvBmhfLVfZNyAIMJ6yGzjrZ%2FDOMCjQ5xuRRdsP7zMCybmlc19zwSUa7KWRCGlLFWgoMiaDJK3SZvlzW1Nn0Vl1qAvGr5cxtrFdGnP6TcntEVDxWhVBNqPx1zcCosfiwURm%2BtCJdWbOkfuSYneL4KHwTrFXLL3cjezS1OUEzljhnNP9AvdxMajbqZceVJc9%2BSjoCEm2JBcJ3wdgNV36QClWBmk7jFvAnMp6ZDQfubRYacunIKXxN3gAbv4J5cwSi0GuKJVAkGt3Kr%2F%2BS4xfpCApV46BO%2BblpMRGUhWE3%2FZxVCNHz5qVA5ITnja2c8iQL2%2BVUrBKZJ2pLUNfTBJQvPeHsdrEchMB%2BVsrxrWMA1JdjvsDN0Lzr%2BREIdlYILtPDlXMG6uSGRjhSHFZ4xxRxnG3OgeJDBUHB5ts5AwywJI2F7Hn4wWxrfu0HVbr8Ee4PBeA6osndj7JUrJu230CpstACdOqtSnQcvJRwygweuJMLH2kskGOqUBOOwN5y%2FNag%2FENjuuLwex%2BCeAQG2X0PhoIxT0vtU2Bu39ZzErjTOSgMvaD75pnSKl428H8xkBY%2FXo1m9sj%2F6HJOBUOVwo0UflXMxS8aUtNmcL%2FMlUBzS1YT6g58TrJ0fhlesZrq%2Fv2dd3HPsvK%2FCqYe%2FFPfhpDlGSA3VG8ftxMfMoHLoan%2FZgtTA3tcfcHLNXt2uYFVrAGNsq3LhBi%2FlOihriWr2K&X-Amz-Signature=3cf0f89aebb548131982bcb2b421987cb80e2875592866a37ff4bb3fc7ff6bff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

