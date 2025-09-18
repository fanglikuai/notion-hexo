---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7ZQ2B6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDRk%2BlIpVD7APlqCrx7S00fcaRlptvn2WMan8GpCd6PhgIhAOVtzRorLR5X%2FAunavVdBKAfZmBbNO3Geo5zgMyHD8bGKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxFFxn82MyYgDqmdZMq3AOitu0wIaW1OlrsdWqj04%2Fe9RY1ed3%2BQkEA1rFrZb61lGNbrQukOXeEy%2BryNFcjEASjCJWgHqmLwxL1N3Z4H8UGxqTAsE3YMGB5kKall44k7IK0X62xR2SItV7sNfIBnY8rEAMzaUmLkXmbyq6MhJ3lSnQxrJaGjP5slF6nHADOjOtK9vSa0X81YM5hO7s6Yg2OLxc3HJsKPVoYlJ63yJZaCcYT%2BADBcA05v3jWb%2BV%2BlPQyazCtkSiN%2Fm5%2FmcAoijYlhRL8z5vQsuwx0AryhC%2F%2BgFH6yyjlrbN7Cebdavq5RF3MlJ6JeuFwdtbD6DAgITwni5edZBWZhGlZyA2PiYwqyPrhhW5Hx%2BCXbEP%2FJfDtbPzOUK9t9nJMw1uPEUPjnUfe%2B1RoK%2FovcjLSv0uxBP%2FVlefFoAVmZZGdIekMSSVRu%2FziYIye%2FBhJnN8kAcra5PDHF43F5As5WBYaRzei%2FiIf5nAqSId0WAgsAVtxkaI7hgJAZqr5XtiGtgIjJ2quSiuUZILhTlVvzw9xYXRuW5rF2WwuypOjOMhiUNgrgjbHTyb3DaO8LdlIFZHeIVfbtbt6fW0f4p3maVZ24j3U%2BC0sYOYADM4sHSXpTLzSnI1%2BBtyuBcSbYaQDEwgDvjDkuq7GBjqkAR0PUaLRg1PVQFBFkM3McsFVbh9OldnGZoY4le4KhisjjTLG%2F9hhW525nqvkzX5FmtU64HDnlR39TD7q%2FxU%2BUfbETSF%2FBJLNV%2BqN1RF7%2B5POGNpiNogbrqQPdTOnEUJ4ezYvOvECGChwGugGcIzQu6exwG59Gbk19RwC8vZOljOaPAfe1SF4PTDW9LEvNAnwWt2K%2F8vRk05ZrscqeR23l%2Fwo%2B%2FDU&X-Amz-Signature=aff0187fda6b5d6ffed6a87023abd13332abc803f6e5848bdad403a3b88d8b33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

