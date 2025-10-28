---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RISG4RN%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDnC6oKVRwGHXfZ1kyS3kF2%2FZGG%2BMPpAEC0UECo074TpgIgMbdMbubjeUGiLCSCnotxVj%2F8OhbOG2G7cu9OxxCy3EIqiAQIuf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPAoUgOvfcwYYvFoIyrcA%2BXUSYzEPPxuVRcMWhxXX%2BPWZ4sqB2SYv7Bjh%2FwGItJjq%2FU7PjhFdLDEg8my1DCIK62MpurHG36Il2tlvLLdHH2Gyvk47UitfzoQPwRl8cHjdtmXcaa13Z7OtaVWNcfWiwaODCvNWv%2B36Li7nw2EXiUMp2D6%2FA0vcdJ8v%2BGiASLIMDGvgWk1UmF6nE1OueTP5zDVfLoN%2FkupDmVNBxe0%2B7ufWqwZd8dSFuk2Q9gGqX5mXBD%2B56CkJFO3jOdPp5o2wJaniofKLzsKDqn%2FbC1j5mqMkIO0skRC1rBV%2FWwBipi1DEYv9szPUea%2BOC%2Ftq7BFKzD6AyTLP%2FYKLfwmiHmDjtWOeipsW18dImRxrBSO%2FXJV0IehUw%2BIxNk7mHcHNZIGkKZOo3IdJ1Bj4qdJwg49JIu%2FIlJp5sS9O7D4QrSmiNqckf3f7CrlSIbqp8yJ2mXcyjZhAzChcWi1JLm3qVdNDg%2BD4u01hdEhRgkTyqstV6wHh6icZHhROhy803T0UbPnb77uXFU%2B79sYwQP0G9LsCXvI1S0fR%2Bpfyd%2BPShJvPDIkC3TeoUp1NgpYntUZqgXlbDBNjJTIVA%2FvY%2B8n50k0nrZJuMUsaxNz4NbhEYAugK%2FxZuIligoJx2Ixsix%2FMLbjgcgGOqUBdevUeGpkCfyN1tB%2BqUSgLdMRSaHovdXcHHgwYvhcZOyn%2BUdN24Pg8d7aJ8CvRLAvK6TZGwRYwZt28tmjRCJmBuVtYWOazMpt2UH6fRUel6tuPC%2BxVBfhwi8OTXHXgK9GRczFMLUGsUaBAanksTjyVxAvms85o%2BkNCjjK84Px1R8HpYyGfNHx7IHNoK%2FPXOEi0ZHI7KKuvRhad2%2BVDj%2FH7XcAtYmN&X-Amz-Signature=6875ada34a909dc79625c5d60562b7b2fb1867f3a4593aa87299750b53bc4195&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

