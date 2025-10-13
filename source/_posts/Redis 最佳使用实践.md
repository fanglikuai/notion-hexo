---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBSVVRMC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGgD%2F5HPmUlfXcEn%2FbeSkg9Vvq%2F2LLjIK54HYhSBV83QIhAOnmZ1w7%2Bh84pWicvHxEeVyaUHQPIjBAKp2bdoGYpNMYKv8DCEQQABoMNjM3NDIzMTgzODA1IgwP%2FBZCmXtFF3AKmDMq3AMMhBxgZ2seESBKKZ%2BjXul6HfWdAuhBcFh5XTjS0wGUKCL1d0xQfDH0Jm2S567Pk2M9hJUJXv1wcMCrmkE5b2mNaUV3Rt6UduZNB6qi8Ha1w4j9hkR7BcEBf85XO3uhaLnl5pGWlT9AVEY9MEhTnahLoMTvrtaM5Gu%2FkmGoHxsCwMiDV3FRYBRdHVUSzEFedkr10PLeQ2DUHGXRNv15GjSNB27f60gNMAfEBFqGX7U%2BltGMSIW%2F8udB59Rg75bkVz3ZMpMa2quz6Cqu8HThvwmIcK9qgAK4HudSOmmwPhikMXdoAX8ZftyLG5KZcW8yPyCIXP9nM7H5%2BFyv1Y73mFvW2WnNgcWaJoUEnT6nkPwvt%2FXccu%2FwALdzN7ps6vY6jerdgLXfQXWfo0Qk31vwGR%2FrzyHS9GBxfeOiE34QlIfiM0ZdQ4VJUnlTiE78ZkO6wsVmT4Wv9sBrHXEnBeDErSfb6MeyvzvehaMqxpaM3Mov2rxLVUB4pEXKpC5tHYSL%2F2Z1ScK%2BbIp0Su2DGNdDgcVEl6mbCZAw555vKGIKSuHi1qjElpjScshylk58QNFG7rY2C0pXRcnSBTCN5wLngCebPYmP2HTO0%2BKFqED1yFOln3qF4IRVbQ5fWwBaDjD8pbPHBjqkAZ%2BA1FalGNhvK3467mOlWnU4CpHGhOzReI2vXnA8ve0oIkZkSGEXpPYHFZaHRVflOLhJaQ1Ys3kvIrotifkwN6GeUSpfpcFIMKxXwpOBIfvoDhkrVUiQ2WX94FbEHBi4F6rgjqKObM9ntMVZUs3HO9hpOZJPftynRRmwTUSnUBJjMq8xSugBFjifkx9q%2FKLW9xPsYspq1mcg%2FHx53%2B2XULpdGNtJ&X-Amz-Signature=f5af32636f58715d371d6c4e0dc799887663c30eaf91aafb29be914e945e97b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

