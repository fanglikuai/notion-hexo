---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RHRLAX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIEUuZilhdogSr6D1mVZlKJe1KEf%2BJKuxaJIFDuQu1fMuAiB5G%2Fsasfcny%2BTvAsdJ4jS1BUbdwesm%2Fk1dkb1SOUvr4CqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPQnYaev9BdhW8D7hKtwDcjvvAiZDiyYbAp2zyxQ921jU6LylnXrxhSoz2o4ZyPxLaEN4x8polasjF4xcCryfsq7ndRYDjjz0oyTBIAn1EQZ3SDHUjbW4JJZplU9dDf0%2B8HY1WHvOHrr7l6BTE%2FV%2FPuH8xPFB02egzu2AMQb47DGtI1SOfbJeJCoROXXYuKE%2FkmcQIG%2F9TuhXzuTzvkJXtE78aJKDgfKiJ1VE8SnAZBmDjCfmu9MvCcZff%2Btd2h%2BX9ZaxTPzuGi3U%2BhRwz%2FwE1TQuSbTWhEw48HAH7%2Bk%2BlcqDGNiQ6Z0twQ3H%2Fwo3wXQZMGAtv8xQjOSD5sBWgq%2BdZQ3KBlUuFVjWTRrAoZP9YEfll7nnq1f02YYRqSs5ssyYdF0aXaaXf8aNWD0ZevXi%2F3OS8BuHeskZSp0GYZp5tO%2BW1EM3urfAJazLveL%2FGpwtNNjYgmBy15i9kl0AyOLHSYkpgjTlpXlbqsVNHU07LI0PEdYRAxBrKyniaAaLNM%2F2%2F6%2BVr7HT1H0YJMLQtcPBHu%2F3Rn%2F%2F56cEjL6xWyA6aeKN0PYBA0BHMAGZRh91DqdGuxZkQSuGWcMAStxl8g45xW3beAU7pZmJBi1S2JWZkWH%2FwowIGTQfwvBQF3QX3xxUFw795J0lYSg6YHAwmdfUxwY6pgH5jayAeBrVEGSvn541Lmdl%2BN%2BWtAbPZDu0S%2FsBwXIIfVA1pHsPQ2NF3pmYBZaYUpS%2F2ZLnycZAK8d0Af0mE7G5C5jIchi%2FYw6JQ7OdJfpGZZPpD8wjCSTJvUDTOJWQp5pO2dg%2Bktw7dl9jJcS0j4b5hoYvRr9imTYXlO20AySpM1LjmInJN2g8uNYZ60QUl11f49E5y8K%2BGbppYOQk22p%2FXczzmea7&X-Amz-Signature=4a225f0bc496eca9c3de8dad07e7b544527c2c72b6f06c688a57679379f78037&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

