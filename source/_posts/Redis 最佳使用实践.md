---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWLAFV6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBN8AOx5t3LWrxzsmGXsCnKEALDhBdO%2BvU7JYZZ6Jm36AiEAqdEQfvurez8ptgZXgDqTzalSffmSK1ABOvH2ZlB%2Bxjkq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDKyXo97XvHCwdpWvMCrcAz5kOoIE4rehas7yU71omvLOd1pT%2FPn%2FqpIhicyOdeAz7sy9JgyRou6fCmzlbdz2skQeVQc2Avv%2Fg2S4GqUI1px21w3XQAqKOjdcQuwsMsYyD%2FOzid4LHbZPAFHXU%2FJEV%2Bh6OhEeKF7dHdmKjy%2BRiRkSl3ULE%2Bss1zCPZ5wXjluMvw93HaBYHLQcNWpnqEmAvu6WAHukWDJa0IdhQ%2FdDMcekVXYCyx5I2JMvd5rnPIctG3HBSEKmyVEDQLC6L67fiVUCYsi4Wmp4aJXp6N%2FHnymBA1O5CgYmS44EazmUi%2FxUYElbyhTbfkk1NAJfVD%2BVD8BFlWfdsTLn4dux%2F5SSh9H6myaxsDRcaTA998jY69dPZOxWca6zfQOcNrHiBWeurimi0U%2Bh6wNEcRprXKsmzzHhbPyptPYUja8Onc5gblDiB%2BEHwZT5jJutef61UP4s1%2FAU%2BGX%2F48Q6pLQ7XH%2FJQrP7A5CjKhlUpOg4PW04HoMhBXIFUkH4o9mQbXJGQXXAEBWiYNYuvsnGxmx%2Fd%2Faf8xN9EGRghOPXt%2BpPboL3j1nDLPN8gMzhKmBqt%2BtyUw7jt%2B9MTolbx1L%2FPuvL5TtfAfGm7yrCZV8DEn%2BNcbNC7cqZRyiec1VpgPYUQnyzMMmHzsYGOqUBlpnhaYX7gh2e5AU%2FSZX%2BwnXiV707tOUEQat63N9rZ2a0kmqpGXSQGsHQLpaOEPUoLdVJY0cilLh%2FDSS3GXZKsPmM64lKcX%2BO6iSHDuQSbt6egMrGlplBHCULo03B3qv8cVLclrAXEG1UgaW4QCiaZglcVZfH0q44KIVvvXbnI6ivQQ%2FYmhF3kaSXnMqYFGJ6X3rVn3NhhaAdYrMDuGso%2BmiuRm4o&X-Amz-Signature=85ba9eac291c32aa922163628226870fa7fda898f625e89dec5402d668283532&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

