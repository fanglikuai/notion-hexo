---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3NHE2HW%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6jMcCFSvgbqAgP7eXfPa48zaOlO8BmFXhEoxj7nYKQAiAkRthYEwXW9XtiiP8TgM7qZrEqPRpvWpLByNf8G8RE8Sr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMqbeyB4Hj1T61GGaOKtwDxC5GbXvnNBh%2BcFF1qFmPxdM7zRd7FuGE8Z64Wxi%2BXh2zPZgjm9XMPY87TKlOF1OjUpzV4GaiPKrfC252I4CQb9mD1IAGr956UypyECXm5vNv3RxWEHMcUc2CdN79qHAglCwe5xCAOYESGdXsGeoOhXmzw05rA%2FutlD1rymHn4dpSMziRbtbqJCXNVirJsVcj5HQvy%2FbphdU4DTV2TBQQ7Dfq3oTwLeVXgJZvpb5iI6sU6Up13KoK0NNuNyoiC964d9p3oOVLKyselVGRBA6BVNnh70e7zNg86pJ%2BiegzwQyYZO0m4fYVTX%2F89XpjWwOYS3j4q%2FkT0uNH7rHcrPJMETVXiSehXGNrMs7Ts82zlsrFX%2Fs2bsmRUoFkwupBRyOY8qeFctDIHL9RzV4tkqOqPO5WuQV03Z%2FajktULZI%2BfgWfPUuAoLmy96D76qbb2cnL5Rer5XnoiqXhyp4HYOLHzceHVGkeT78pGAxXoSq%2FviIGIfCaLkCwdyXjXXEwBRVKxOB%2F%2BTDUghq9nip9xo9t9T%2F1ItGPzMh4UqbHrGyaRh2lXjBeOQGQlVCJnUmh8aduqjRKFTLpxSt0nnnAkrZPPNQQfahmzBgBuazmDZru60%2BNCkcxNCk086BdEhYwzpyJxwY6pgGW%2BYhgdeNBpChZj0bC7s7w%2FEfbrqwsjXHlIXBTP3Aeb2iT1xFFGa31nxORWvPSJ%2B25durEGaZZkuWSbKiWe5CMDc%2FZUiqP9ic%2FlbF6uhZGthgObvKc23VoLefVnjLLQc0AuSfIOQTtpORDpWAQ8nexn3A4DWw2JuKye5p9aSFbhkjtoFOzGKKsRMtR3kEnF3Qu85w2SwKRITGxoyL6CNibK5I1aGkv&X-Amz-Signature=6f67c0d57ad06a6800ff6518630b8697b57f255ec59a1c0c0e76524c58731f4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

