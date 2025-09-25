---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTZWPA5%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDbVnYgEAVMy110K1qUFDpXcuAfaRgJASfT70C%2FncstEAIgA%2Bo4Vy3wdTykW9zCPSiq4tx%2F59sGeLhcGFLGGpRcUiEq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDOMfvGlHBLEoQCfKpSrcAz%2F8mQ7OMohyqvjo%2B5JCBx3JTU3bkBGmi21%2FcqsfT%2B5XSDMiCKr7ySqEGVKkm5hHYpd3T3FmHxotuMDsMC5%2BrdbEn01tfxrICApQ%2Fl1jJffZffI0%2B7kHjtpHxKhu%2BGVVfRzHRBSxowq4gbYLtFB2oqQg7F0pI%2FESiGjhICQ1yQSfSYtHyF%2FSxuHjh3DfnsAN70MV7FrvrHt8vbsDpQlQWSN1Jn9O0ONxDT3uRuWXZqxpfickY%2FLjYqgCF4hv9iyRLVGrMKdbhOKEbEeWHt0N5JIiofAQCCWQUDcJi5rwBZDfIIW47106yetPdYwi7svh0j3oeRgwwnfsqrGE97pu4JzoaR7yqG1MEHcox8olEG2rlGVzrvACHGOh8DNCO36a1HkwT1IyB9rOysF24L3YVg4nWFXy1%2B0NFc36pn48D6BGrQ%2FDh3evPON9Ix4%2B8qyL2kRvykFY%2BqPBZ8dThRxCKYFn3tpHXc2DdFfGFWc3kl8GdRdTeGjzz5wtyui8c4ddGAuemHU5zci4pL1CSIJrqqgjV033DtG8itKk%2B%2B4IDuSJmUS73G%2BB1Yp6QvHykCLskrV%2FjMPdK4XYEe3yQ41H5x7Rt1Wz9sYZNv34%2BobnMZk8G2AGprVW3XHWTgvBMMWF08YGOqUBEiuYfj2Vn06TS8czEUmw%2FdtWoXCBBzVzdxibm6pAlEfntUs3xvAEUS8WJlTfyXMyEGJw%2F9al3vS%2Biopd4%2BIoB4YYPkLXqx9lEWsxzPYAZOjtUQzF%2BZ63K1p594TUI4tZLIEpBcEcro2dA6XLGwG5Vq2trhnc821OmdFhz5l82%2Bv3OT6GJEXYetConFNeLoW3d4fu8vWUpRB1uvSkVSrYZ1jCqZBN&X-Amz-Signature=d695b73563376cd80b3d348a5c67e592ff04f411f863fcb653bc44ddfab0f1a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

