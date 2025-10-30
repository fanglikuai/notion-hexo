---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS3LAL74%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAW6AkXeOtSQvvWfBeE189pJzfx%2FIa%2Bmy5oHmtztU1C2AiEA6OvOi1kLRFdT6BP3QmS%2BT4ih4CH2lka%2F4JhC8qNBtQoqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHesuP9ZdIz43nhpnyrcAyYVLMTtjzzx1HIKn%2BgmRl%2F8dInxpwgGllrKh%2FZIjuOZqr%2BHgOLFB4HayIQbuVs8AG8Ad3LLOo7dJ4Ip0GJNTz99HGH%2BGMnUvI7bvB8R7Lj0fr1xuijbU7p0iL8s8k6CNckgvpbfxIBhbUMV%2BMpRgRSlyxMDGRfChpH2j6Znl0YjTeTCAi%2FEh4264nJN6Iw9hAFmUBNRcCrKyrTK1m8PpGP7coMpCamD8clvqOT2ruwZ2V6mNjY5cVdngc%2Bpmta%2BPUKh3%2BZ%2FJf2x1Bb3qhN2N%2FQr%2BhfYnk9HZ1T0ex0WaHAs5Ofnx%2F2ri6unTiapDj8vQyPVQ0b5d3lPsJkXb043RawnX%2BhhCuclKDS8%2Bxgbj9Skh6C2POP57iTSjx6mSE59W248evSYID%2FrRZPsoIkLhtv7P28HrBd%2F9%2F3RkcPLafVt7wQ2ErOSzaTE0JCEuvIUl7B5viRcwUYQ4n31goIWx%2BR4mZeiwuVEiI98ceEC8eRU9AQIO8yEhhStGud%2FMt9WohYdFr%2FRByQjlysZlnY5xBCdyl511SyYVV75oDNTJpDS8pnvy9jJnLxjXtuC8VNEhB60UVV0UBa%2B31K2GmQ0cn0t9yIg5dYCn6R40FqlYtS4zf1A886OiGGmAid7MNPPj8gGOqUBqgAs36rkZfsss%2BUk7QoEmURCfipYsIS9sLHNO0p2IQg5hpFwz3raJyClfjhkHzOy1wYKms8ep6zz%2Bt1bepA9468ERtP554%2Bww5svI8gE6NBRgUso%2BLLEZnpPekCzdkSrxoj9CqWuHtGlZU3r7YUUaJdOeBGxzfYFVGmrxclOmHZf3D3FT64vdYTsccWRiezSk4D%2FJ8apScudRjywY7k%2FvSVlzRYG&X-Amz-Signature=426c855e4a58c3cdc37f37621d285f3ded594af9632a4faa27258044d4075af8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

