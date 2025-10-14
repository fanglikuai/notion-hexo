---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3ZYXYEZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5RioBok13lnpoiWqRcVxVM0ZmZ7%2BfCQuVF2cvukdq5AiBdhmJPKwLmuaKmGQfZ67yRRjyUY597lzZD17fuM5gz%2Fir%2FAwhjEAAaDDYzNzQyMzE4MzgwNSIMBFpe4mrfDUojPwQGKtwDO0UJtfvEagtvZFJBIDtSvy5jH3qex%2FVX5snpmEkxAlwCbCrI6hQpXfX5%2BZ4owtDcFnMLQlXLp4VcHCs%2BfzmDrVZIyq0T%2BTUe3eUX4cgqEJdFLpVprV2QyhPrjoF7S8mfLp%2F%2Bzre2NPx7AoiKfLLOTf%2BpPw58l9W9MmGPB4dxzZDMqWYWxlQ6bU0q1f%2B3xxw7HRLaXT2ghllCaBuP7zcbhxv6JO5USpZgU%2Fw7KJ3Um7aDLyKWuZjsn%2FlAUbvLohIm5Yen2AZC8nsV%2FclMDrIM2PMMXgGA7ssqT5nGsKtg4LTroe4HXcfZglRIFPlVqZ2EQEcWDqKvwbnOz73D9%2FGgiYar21StazGVZzH8%2BlrVAqtS2FOXC0T2zZBw0kqJOQX8mXJXl8mFUjtdP0Tcmd1dNRs2ochgMx9CI1Z%2FRqbQrcgiDT25OLP2RFkmhKDM18av0UbpGNehtzaoumoiZmbuVenPus%2F50wJ1FWkdC%2B1YxQr5wsPLm4zfvX4BJ%2BPq7vIdHfAaj7vy44IdeliG6JI0u335iEwWqEuFCcZjV29AcGRweXZUzLIAOPcoLIIBJBE73vRNlT%2Fxzs02GmA58I%2BeU5Q3D%2F4R9HnBN1kNH9%2BN25m6lGZsGi8b6xNNPgYws5i6xwY6pgF%2Fv7Abx8Ps7a4bXTdNfA0RT%2B0Ud0zHOwqDzfxCc%2FqKnvrISn2DdwWTLZbDC0bxese4bCjfKkF7hN0bWwiaUAoe5JEYvJVXqw3GEUyXbpFtcx%2FYnSqAxJ7oYqprUMCWhulSVe95qZvASFe7LNRPCusUuWpkAUWKEQSHorXGBAbuaHpa0jD0M1YIbVjCIexh2GlOHKEuOQs%2F6SGqj7zmXH3MJji1QGQ6&X-Amz-Signature=5d929df7bd1d965938d0a5fc4eeacec7f4c03ca9e5cb3409f33f59d79a30e52a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

