---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZPDMDJN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T090055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQChzKR3YWhJ37EJzg9aLLXf8RECQw1khLdSKjeEoWpnEgIgeX8u1XzKfH4SE2YbLozn2aRvLluD69lbDkPY6qcpBcYqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABHndnKmizN%2B50uECrcAzuIVgT8%2BiCjw%2FUd1Hd%2FiQVL03Regs6aw6hLCeCzA4fgp00B0vuXMfCOguGucyCWNGPD%2FPdcanNmYfUvskkbMy%2FwV%2F3k6k9HzFEI3c11S49i1v5HhK9JRqblECXuI8D%2B%2F5ezPVp%2FFBFHEVgg9EURC%2FIyxoQayhruinRRZAPu9f1GtDt3%2Bu%2FvrBPHSFSn%2F31WPinng69y6FsGveC5fOVN5QvB8U88PUcTCO%2Fv27UpgOt8IRvuaJqOQVAPHdAYXjUh7cd6ehngjbsTxTIUdvBWQiH7l4swmdZYzPFhvSbt%2FErHF7TOqPUaWHDQ7UuRbwPIjVv8pPECm5I0xGbw5gZg1Kj2eYsFfi2T9SrbZo9414uppxTnzVLPgrf8q2dW13cL6nRMzdLX0LcQ6T%2B8n0l7r8oau4En3g8N%2F9AQAZlQjebTPX6976rIkfRWwkYfN0YG11OhwNRX8Sxvt%2BxdQFB5W4ckkFCn%2FyzHLPlYuG4bPyw0kn%2B%2FilRaeNlFpRR7IbV2sniOHQX4v8AdRAAha8Cb6dmQuxmxmG%2BIAdQkUs20ad3Are1mZ7HUKP72sRpVb8QD8m2kIpTlja0fMx5gZo4Tpo%2FcqMdmA2KIAwlV%2FNapx7r2KjcEWQkM29NhuRuiMI%2BTk8cGOqUBvXSrsP%2FQpZhYJE3GOVUy%2BCmcRg9fLxGONWMUGUvYPk%2FEi6T2Ds1X%2FSZLoVhIxiYvXGTTfukNZUM82RvkleluUELVjBDimSoBYnntvEx0dRHHJDyTPDrTYhsx6wVkpRuNuEOdJYUy4DjC0baoo5%2BfiHLntXRPVkn39SYiytnJx0TXcdzUC6NvFJ5dOrCdfv8FTY3Tipt8IBMx4QsOCp6gdxr%2Fw6Sv&X-Amz-Signature=8f7a79e764d1ad9649a90fca592d47579acd2f711eba3475574f77e578cc4cad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

