---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7VDMU7B%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqooXaI8hmtb85cX5I2N%2B0BxaXQNAPitHvnFRgPLDuvAIgHdqk8ZYBV5xgTDiSxQjQlqBSv0mWI3fIc4QwC9tguyoqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDORE%2Fz7YyQwTBpm3mSrcA%2FOx3QGfbG%2Fq1dV1Nm8%2FXYEu%2FDVeqbES2aZLKjKTOwS4xyiCH4bku%2FPKtdPThP%2BvHAKNv7b32nrSooh0ERl4ceSHgXksDqjNxTnFAKu8LKSVm8Gy%2FpfBByr8AR%2FrVeRsAMX7uwZhfXWCzj9Wb0NdCD6IqarE1R1kdiMBiq7lNFxsa0TymDjMGfE3MBIu%2FtlQ6y9iBW6LjKDa35vz8VpPRWh6HXRFTnXrGu26Dx2Ty5yRObr2SNI64iAmR1xg5p2YngJGXcpAR%2BAntu4Ewx%2Bkx9TtEQqBo6cpSvQRjtvw6JZPiMn4QmcbjTjKsWwDTreTVZ%2F8%2FP%2BYPLIzb%2Fi9fvPT71DNepsje27n4%2FT7pPyrnFnmmcE54CtjyKIo%2F1tTXebEQuFqx4r7YdrL536CzH%2F8Cf%2BlgmUgA06RTMc98mAjvNl8tsBewBl4X1dNHQXscxFUYtzIp5zJgux10mU41giJn9PoG9xVDKTVja7tKtAtqV88xeoojmFehrsZlOxaOavh0eVc2MXcPsP%2BnLe4ulFfs2J22grQZQBwi0e67I%2Fb5gfnrY%2BWrXhaCbq1i3MM%2BUJn609K%2F7vMLZ8EKI8iXqjCZ2WYuuxBnlIvOmzgqPNamD1ycx%2FeuWd2k8w1hmUqMJrP5MgGOqUBNv5hcho%2BI8G%2BaH3LI8rsvT7r7eJWZZdzS1rGZ9Ml75NwgfymsQ5Rdg7DvjgyOTauBqEWgawd%2BCVdD0ViGLBL7LNWlU91NVVtlCwricb48ee%2FUDvKhKCsQygbYZu7Vdt%2BUacc8p2ymYrBcEzrBlqHDv6pvlMRPPhYLOKEWRHIRk4iRXV5xHvcvhF3tLbdFsa2KrYdc%2Fl8Su4T%2FofZOFZMfvUmdmdA&X-Amz-Signature=704d52810abe9403281e8ba340bed1be6d488866f1f035fbecb054278cdad516&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

