---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCIPOSRH%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCyEv6aAVkkZ1WH3WgJS%2BJuSj9JMATFoOsOSrevmdjjagIgekUQhpvuc3ty7oAhTaIYkXIadvvhiS6QiTMCGIHS%2FTIqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKgrkua09hHgwnWlgSrcA586USsXUfEvmvRKfUoma1E6pT0GT3I1eaB%2Fetbp7MHkmpkGCEKGyQq6FJRRlzE0Cq0Z0XRBaqsfZtN5gyjxVlxjWRslB5mNASAURQhQKeGbBhidbMz1hpE34H9zckNZ5CbgnCfuXwVpnl0SIwIuv4E%2FUs2kmYDQW%2BOykztTaUBxbdJ%2BoNXu2ZtngqjXNr3zPWKsEqo98hK8Ky7k7Ih%2FvbhWLj44%2BcpROCUd%2Fs08vwRBFnioHT5tTKYHWz%2BIhxoBDVxpHjRWlh3e%2BJ1vAc0tWUvZGvrD5Ie%2F96CDL6p%2B%2FthTNFDAV5FHbfsiJT%2B2DXPcP366V20DfzTWZHra3t2xQjmS773iqrFgUeQNi1pdQiAcQ2P1Clc%2BthMk%2BQ2V2Jvn3TBVTHla2D7ws3%2FR0Jjl5UVIBWrM92QXsodJbBHKmrCCrxLJULwWDdV5jQL%2BAhWhULIswJLrLZClkCO27U4Z5zL5cfgZmCtwkEdT6fClQlz1mZb1pmpeJw0IK0dt0TAmJ4EFE4fBPgOE3xOUlRFN3kjBc2r3OT6601Qf%2FPpID5W5K0ctxiYZ45AAqgQtnJwFx0s%2Fg7G18Hv8V%2FtsntBkcQUS6p4VbyxJjAsZMJckWkcIZ0IVboFV6FXnPmdDMOmTrcgGOqUB0tDWKdBANPNYUrMuzM%2BZ6JXmkGE965%2FpAKWh%2Fn%2Btir%2B1C%2F%2Bl9H%2FHMtcnfW5SNgl6SKsUdtokXng3XyEWqGBCV1%2F%2Flz2DjfzsNBTIQuaaq%2FAG4%2B%2BEmtzl8rZJZBF5PdPYGr8ZdekXd9g2UdkkO9DK3dAY74zwKXdRDFOQR69G0Mnp%2F2KHx%2FZyJ3cwxALM81mOw02GDBaNUFmssXzsMfRYyTGi%2BBee&X-Amz-Signature=577263800cb21fba10404a087dc92455b082e5192afb4d0ba61b647e831748ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

