---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM7O76AP%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDUx6AZKS9z2xAnX%2Fx0AoHLYUAXvkNWhcL%2BKl1SYpwfqQIgXW9cemByUR%2BgwdZM%2Fw71bxGvImW%2BlWi0iNJg6OLcGmAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJa5VLpKuFLvp7rWPCrcA0kY8aK6Fg3TXJpmt8GX1C0%2Fuu5kdRvm%2BUB5UZXARijQVsNyRVxsKokY01kVsJ9KSrRMUXTDaWaXN3JCGvc%2Bs%2BpyZfPpjPS63t1xo8uOxjj4nRVIuU6M55xUMo4Fh5VFLA8kJ2bWHX5fMyjXbV2oGfvS52pJEMf9klKANVsUAUO65TjnE6nuElOCTRznzqB3F4NAlyEPerEDOVbRqPZmxl%2BX7LqScrJNsLk3c9VJohaKnx9JSKW1XfYHU%2F804%2FZR8cHF98Dfr50kfCwKW3jv1nUimLVy%2F3UU5UYhggVH2qYg7uatIbUegMdgok9Riq%2B%2B6VpZ252aAhtYxdAiYOJuhEejXTEdMbcovnW%2F0IV5Odqm0345UqY0lU23iUfmDcOpTseH8Tnr%2BWCk17Ibab6k9t8HGuxC3HP9wpKkdci%2BTAdrVk1dSus0vt%2BHnvP5P7BobN58nHy3z0EgBE0dv8niJQ3Cg1DbVirtFhzeHftdp3IKtJ7upp5lphqvdgAeXD3B5x2dm1nzHxeN8SHX%2FITXu4%2FA9uAvHQ9XYxJLylKsqSQPhF5vQv3D534gOFInfDpmwN4hzT1yFx9L00FVE7VTh%2BJZIqnK5qJWXo%2FzB6deNjSp7%2FniSO1qYO3xbo9OMIfhy8cGOqUBpbWBABgey0OuQLVj3LixrC1LHbzF4oKpe8oAmV7Wbcn1Ct2n%2BmXAjNcPCOfnDNlZOPUjHf7UQnsfqui%2FC2Y%2BT3Y9SvLeja376n0KBwsXvKi0%2BykwcxI%2BSMBRuc43aevl8XQqxtvcqWSjQ2wZ9Mh8wPTeVLasqk3YWkYG%2FnMoHWAEgl6Ue1gSfw%2BKHiRpneowEPB%2B88ehUNdr09f4dtaDeEJDF7wA&X-Amz-Signature=bdb1c3bc27515c76caf14aad86646ba60699c5e4122101095d5ccf101c42332e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

