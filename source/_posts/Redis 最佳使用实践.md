---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGQUK3M4%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQCqkQfxjMBaB%2B%2F4CZvp%2FvzxNHuGXajc%2BMg9oJE7Xub60gIhAKPG2H0s7bG%2FgxTUHogeT%2FzJl0RNf0aBOljaem7dtqBkKv8DCBQQABoMNjM3NDIzMTgzODA1IgxhO9UhfDvEpG1VoNsq3AP%2BLaVvfKhQtgbeNxVsvyw4CmXDriDtEygYAKAydG7yhXKO8AbEuE0xnFRUSzhgQQH2lvb6fwkTEFdvSgMiEv84YUSkqs0DYceybYtz0fAuDWN%2F9msOPA585bTbDmHcbAKN69DPdG81baMpEDQRHyBN4%2BnBaxDZ3XEr9xlwBo4wp8XURUS0Vo%2F%2BLa2HfloKaC6PioxnxMVjf79otLrpulPVun1xZj6xPb8yDX60SoMGRyEBs9Py7yy4P9ZzCxDrxIWdTJWj9WbrTviamOnR52sfpjEVCl916ZwoVxjJIG23bju%2BNacXbj5SwbVlKkNaiK2MiwN9lzTC20UL0HZMib3rwiV2wK2CzXZvJuUGbtBboGeNpIItat2jn9aO2o1cjmSwGRdlZG9DuYABGcdKbkQStrr3axxHUVrrbiXlbo6itgolSFNYR58MNO4GTUKBz6F1HmHOF1oLf7HvKOpIiIHxDllsmg7pwo7AIOmVOkaqwMAnBt6LWg3QRv2RDU2TWW6785Y8EwdRO1UwvLCB69zkzQp3JfcQNKYcBAwFvmf5RFzh1oS02b0N67WuUzQzC67mW9oLkot1ahMpjt1g3TpEGTa%2BFm9rCp2WWJpWhh%2B7%2FhQ8tnxs93EewzQbOjCh6YLJBjqkAS9FeRiQbKBS9MDpmgQk96LZx9aH44NCaJN3AxfQwJhpyGLi6Z9fr0XagVD34HA71FvltZJysrqik0I1FkCM2okvPxdG3e2EIo6%2BFsNmBTdtEEDX%2FCx5H%2BQCFl7FiFccUeAQSAvkby%2FyE8etuxjJmluTZ7TPmC4ZoW6Bd%2BMZNs110KY3Y5hZ5WmtbrqEkWb1DUHlwcZQL1QHlkY%2FGEiGW%2BR1M3Bm&X-Amz-Signature=e5572e719573111e8a9401e10cd2f135e2a3b088ce0cceca509f7a15495c1ca0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

