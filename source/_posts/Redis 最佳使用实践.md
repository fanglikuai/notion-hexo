---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7NTRTZK%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCOvn3vX5gz28DJiOcx9JN9yix1fsogpfImJdIV6sY%2BvAIhAPCzUCFbFLG7D2lrvLXNkD6Xdbz2oPC8ieAJ23%2B5%2FYRPKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyfLdgTtpe4mg4Q12Uq3AOINgZp9AxkXJRnPE%2FvvDEVKxuzeiZ%2Ba28WakPQ4Bp7s1t5EtrwuDNSmVEKNBD4UOtUmpLS3KajmvQIH2zfa7GwaqQ84pTRsvRJ3K1jDB1%2Be49F9NX9t9NzzXMCUhL8lY06zAnhpekY%2B9YKXnEvibYwoS%2FWePE6oVhwL58qJp5AR0q4iyHkrHyuKZaOSRrAztk1FkfZnbtAU21F9Gw7ZKvLhL3MrJW%2FdVbe8l4LwljSF3ivE%2BdUFAneKjn%2B9PaIg%2F6ZRV9gw5tvWfLsUiTk%2BTVarhZ%2BISKb5OwNgUSjYGjwuWh5Fyh4nN9fniaUEILAFqlgtLGntPwsP47csoyr0u8hQjBNQqn6%2FN9CZQuYv%2FGg7u3VzfTExA6iR00jZLfJbDA%2FeAKLzvbJI1l2z4p7KOBcFcIvU2YTxIgDM9E4XPWLFjQE7DkwSadfgJ8vg3a66x%2F%2FvUw7CFmBXDTc9e0MAcf0nyWbK%2BNL%2FaNsIR39g5lnTk9KIJMda6FHRKeyYOg7JL3%2BquaXjNN4F2Tt%2B8D1SNfDFOSo043ODITEwVTVZKc0nEN47VpcKOqRc44yKu%2FroOIhj6D3wylODW3%2Bcq9eS3nCA0PyNGmoY2ko1gmWj8WMPUYZx3pgBVFTSbCq%2BzD85YLIBjqkAbXhKg%2BwvuMdkqw4BOFnNrguT5NhkTY4%2BSQFFDnWv3wVtVkNP%2BXoHYBA%2Fcocx0zkChupnCwqvEan7lDafPtmLznZ1aBulV8uSMCMkxyLzVBrnkM6bmL0kW1Ag%2Fd9o3H8nIlV3nItR7n9%2BNjzwukfUEHIQQVQkYBIaH%2FrUgrVNpSJ1OfAvBhP4Xn6jsAYbks4lDHbwnd%2FS2mqks046na892eqsDp%2F&X-Amz-Signature=aedaebb1e7ca5c18aa75e4ea188d70b8ba74613d2792c065e0579e60727b9538&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

