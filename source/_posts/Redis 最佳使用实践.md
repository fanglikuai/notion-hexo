---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7ANN7B7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDMT0mkuCdKtxFOx1Spcnn3uRfmW6Kkp8cHGh%2Fwwkbb%2FgIgbtQeBw1RgyTlCtPMj4ZgHfCnvrOhmA1QDTTl%2B5iFYEgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGiFym9BKN84MX5zMSrcAwa%2B%2Bfwgh13I9bKNTGd5t5YYMW%2BbWN4Tp8ES4mEnx35DrWASPCK0bqREoAuPabHvEOfJwAKjhWnDBG%2BtreRrzjjK%2FGZrHkmo%2Bjs0wuwXDeQAqxgaiOSBnpLlVfHkxr9YSw2xniB2ywLQHRYmW8FF3X0Ujlv8dCe0zg7%2By2Lrw6Jwrief69SptaNvON0jeXPxBU9PaEKWqTEXNjSqP7WUvmZl2g0vKHCY%2Fw4XUI4KlyepdG2Azx%2FFVBQokt73KKVU%2FmNDIM4iOb9gZ8uTinPyqa2BhHMZPNdBEIKDy7kJlvMDQ%2BBqRoKVaDcA3riTRooqxNTeTBvoFhzJQZhSzU7rnRJRFH7mUNmpTFNaJp%2Fi%2BFQFm%2BTwPaNn7acD8QXEHnMAnuJZ6jHRvxL4un8IvyAu1eyIg%2BMkKstb7EhOEKr7zNMVVLxQWLg1XqqFtG4irbNlOTge5kLCpLxLxMQf3LtzVsnxrUjUBQyZQAp8KBiuNbMq1HvutDinKn%2BMpSfQfLPoIV9PXW5pCQgTfO3rbLLjEaTvFsdhzd8%2BAmSF41ZwsDnyQ5uGrRjt1oFtdVTLjoJIUqmruIgtN%2BZww88XXNmEPuc9osqgWDrLJ%2BAw6bSYUxqf3xApPDvIYLZ0RmxQMJbxj8gGOqUBquuyyqM3plWFpG7UYHC7nRW1xKfVc38u2dFL8%2F2rNc7WrHizvF0C8FjhPwymSJo7JaS8ElX10TokJKtghOX%2BKkR5%2FaRa7noWCfJZk9kz64Iamf4uYGtUkclz7da%2FdmC%2BRjEqLNgtOzod5Mw9WhsieAcPMhh62hrzd5zElcbP7rn9o5IUY%2BT8%2BxoiTRYQxJ1%2BgjVr6YkIF0S%2BxteLhYM%2FpEYwFSHG&X-Amz-Signature=7904ea2998f6b40cb5c9c47853ac89c666643367161039e64490054f860abc2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

