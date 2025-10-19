---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5HQ2SCA%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIE5PWwUeyezTY1qEO8hvl5AwDt2lThh5NgbDWIQgc%2FiFAiAV9AQ8StsUVHZnwdusRn%2FqP6RYJqW4mfufPPOTFSBQayqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMq8MxMNs%2BcJYl13RRKtwDeSVsnYqo6wLNZjL0T8NQUaRe5tdNXG%2BQH91rh%2FkEAmV0pyXQ%2F4eaa9h1AZQgiSEqNUyK0AttOYBjVAu7cHHXAFmapwCYGi05eIfKbI9Ng4m7eY357q7UPJaJ3675zt%2FJspjYRNEgDwLTOuiZ6aigzhQoVAXytaV0ywtvDpjAxUmgC0ozOzzTetZCY5EAzs%2BEFGoMradZMxNsA%2F4XboUus2NUzmWtehmL1VhGC%2FYyJQDLsMGVPxmAiIbdEQr8%2F1TVYYRsym4zM914X5vrXyVAe82VcEhJ2yVnxOyUExrSTBLzQjw6MsIVS9KU8qMXRZqNsaSzHrZJi0XUe%2F5HNlkRPxEBeFSu8%2Bgb9ZILFmq%2FD4%2FRMRvIQGcylRKtWbxnTzf4OfD%2Btlwsx1E9OoAti%2BSulq9vtNyW37gx8LIR0jgDTZdxGdg5FgsbGqUZH9Whij5UYU6pFLWm2doFYOoY3aXHRjKzICx2NyBQkWACZYaSaslfibYh7n3gz7i85bVsTkDa9QlE1ljbMDrd1gtOOHEhX5TopMykXO7D1b%2FV1gOE%2B%2B0FSXQlknyZvH%2B%2FrY7ce4klNuVHZth6%2B9Ecr7zWlEou3KDcty34f6N9yPYekuY1tPEZ1DdOGbv22BmmOi8wheXQxwY6pgGibbSmXrCacGGPhTg9o%2BLotmFu%2ByS9%2BAWnmOHjwrAH39HzFy4uucqFFUOw6%2F%2BAOMVX9whrxg%2B8i62iDMH%2BLlMfzctQrIiMiX%2FEj3A0zcpNF%2BsTbI1zu5sV79bza7n7kVnqTJ33MP6RXbf5zGhGAy9v9S1yFQNzGLtUfQD6%2BoOpTA%2FcwsXKhmagvyr7Vx%2F%2BwbEVPU3E19mc4pqqUF5y1G2w%2FACHcRjT&X-Amz-Signature=0069ad563b75eca0b9a5e014193209a3004e3966d32efd0e0d71ce37c09ab3a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

