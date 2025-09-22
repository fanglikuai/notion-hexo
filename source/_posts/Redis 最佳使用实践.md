---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OKUQCTN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaTj5avkVnCiEpuMu6r%2FQJ2AgICsO1LMdYp%2BcJEUEHRAIgB7SND8kY58V0Rs4uCZejsqM6kevLMdA%2F07zyXOPELn0q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDA2Z%2BTKPabZeAKLeeircA3Fa0ugxzP%2BD6iQ31rfgkSaN9CBwewslCFVgy1TsXEazu8%2B2aCgytWE082N7JgdzlZTUuLaTRmAqkfe8J%2BWbhy1pd3VfLjS%2FAfOayy1I3MVGZ9W617f%2BfmyLS5thFLyhCZjwOzZ3MK2ELW%2FOZCtWMiartc06eWKXwT6boYNA%2FIXUTLug%2FyrLRt3fcdGL80Xpnk615wV4fSlmWNFZ%2F397kld7Sa3yF7UdrdnyrQwGonC7as7MsP%2FXwz68soJRZQwFEYDarsGVJRjHr1ubJNz%2BMKIzuKdfFXCbEaOjL1PRGptv9E%2BIUUW7wn3b65Gtf1zTkWlOuq9w8jo4io3vGK12czjLDx%2F%2BtJxjKeLIuGTtf6m4go8hMGRP5lDl7YYbSfyqb6P%2FjIoPKLcUbHAwFYfWW1TCXSpDR8pCtIpdxnD5%2BrIzpYVGjv5gDMUMwSxtSIvA%2Bf6q0iz2%2F%2Bj6RQv5D1cZcju%2B7HD%2Fgf59Hwo6wLH1QTyF32dPhn2cA%2FgO54P%2FCvUTogs9MN7EuiFc6LWGoYmF0ha5viurOlPsK8Z5M4XxR8OyDKdrQv7oyDdPyn4Reo78XjfOOdWJG%2B%2FR%2FTeICHxtvWs45NAtX7ERb7%2FtZHAy4Jh3WRPArYdG6T3o5Cu9MPPhxcYGOqUBzzk5by3d6ddbm%2BFTnUtZMm%2BBQhMvE%2F8fD%2FpE5xeJt494BXTBembjgVnBkmDsLE9YFXkt1AKbtcKLIsKySeyu%2BNUhGJZOgEZcJR0rfhIGOG35OhUEqrMVjdeolpIX4FUR0DPNmWLFOStfum0AEXOlJ1H5AK8V9Buzr6Xn1WOEpIcWa%2FAYQM%2BSzV2QOmbVWgDJRq3GsuQqDlBPMgkaCOkGF4AxtoIZ&X-Amz-Signature=9d650f8330df183299ad3e709c306ddafc7373b1bf2a0bbcee55fdd22b012f77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

