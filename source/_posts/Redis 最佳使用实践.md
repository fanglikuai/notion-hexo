---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJPRBO5K%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJGMEQCIEUhS3lwQrq6SMEYKMvyZa7PZFfZ%2F9mDSy%2FMz2qjoLUsAiANEzsnxicyxF4EivMnmPpcnGfsVVkDRL1hoAFCYE6lKiqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdjoYkdTjO4CAfa2EKtwDEpbP4bUYhaADqitFLfA4R%2BN7qVbahakaiTbG%2FnRld4IBKEWj6v5TRPeteN%2Fwm2icCevVZdsiR8XOaw%2FEaRxvuCUFX32Q1fyNvUQTbvxQgoq5KHie25wX4fcyAZdh3JSP%2BvxBW%2BGqJYJwSG70I9MDMc1ATfALw%2F%2Fd3w7Sl4FXS7fkZlKYVQVmwapGBKPM80kI1hyrI2rY68ohU24mYqqH7cjYGdRa2RXWNzj19otDcQuleXpGN0jck7FQMJLMtzQq84ZD%2FgekzUbN9LeoMhXQQ%2FkUojCzKR6smXisVMEHjmfYVrtIwCprlz5Fa9dSsfx2WbTzuf8TIXp6BzaAI7Fq5ht1RUSzY9i6W8OKDxi2A8gvdqk7G%2B1yPOEiRDAvh%2FFQS3mmUABE4BAoRiULdJI3JVWK0i%2Biz%2BNMWB9iF5f40GBQATIzyCkjRtFIglND35Z%2BRfu2xUNf397Fic2L9zy%2BMmGtHlX6IZ2NuqlAT1ivotnYPZq%2F8qSdh%2BuMl5DhEgZtzzpyYvRcNv7dQ3Ew8hzHwyepLc4Li3QDXahUEsn2QEQGpAZ1aS4OnA5mvPSXJPB%2Bi0U3PsrU9NTeFbsr42%2B4hhXAcz7RXLZwihzkOSyII9tVeqzUgZBGG6oBge8w%2FMCmxwY6pgEynj4Z4gCAPqKZrr86dE9m3eL3eRCaeLAe0c9bQIN5TBona5XIxyxH7E5Bl6wXZq2I2%2FQWzS4InCtj%2BHnvPXd8Z%2FQeUHy4EUcGIWJWD3OYA5Caqs6mcp54%2FYbVCTe53WMETPXJW8Qtmm4014gX7JTUWC9vwN5sDD39QpQ54%2Btfn2qNmbcCj3iTE5qdtSgyUdTgsnnPPODRZrCyKs%2FmPzXmPNqyDiTL&X-Amz-Signature=8be391efdc6a316311373370835580b24c16fa417bda18f65709e787e257c775&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

