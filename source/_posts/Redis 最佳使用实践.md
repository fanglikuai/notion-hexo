---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LFTDYON%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T040037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDF%2BkHucLuOgC6FZE6Km2uYCnTK5H9TCVaQo8NRDJvRFgIgVx9jWPLyEqZcnBd%2B2aQe9lMSZm4l04CoNFIE%2B8JdyRYq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDN1mS3gLFyBFYPzEMyrcA3c0GcH8WPpeIkdTQ34%2F7M%2BSSaFpjSaf%2BoypPm7xkEN8ROxAqAUTsdhfqEtbsBcT4vImhJxrpfwJkKHvzTkEkqLIQfzXEqehEj00q42noKJnTOxjcPJ0hXtoKn67nbfjv75lofXT9UV7%2FwG17A2M%2F9oRAMfbj970IOfOGFsxFB0gfZOkwcFCat3tfZ7snQQ0a9DX9HfbubqHaJ1rdod6wvvZOvx7GHuhrOcoSFJA7N4M%2BULUiOFs0xhMlpDyPe%2FExGk%2FgJiUqYcjRiUP85wqdEFLWslZasCoutqAnYs3JpXfZ5QLa%2BDEHH6AAjIu%2FgnXc7SMyl5djMnWwE0eLThLS3k%2B9ncC18Ow%2FlvliepkHZz3wuD81xda7imG%2FhLjG3mlsj7TIM4Ba0yxiucF7GYkWpjM6D%2FNOW7negpsfh1BJhxGEVhlrXPtT3acoapqiMJwLL4ixTzDrxdcgQohAqaycbhBpl9CjS3tA%2B0HydNFzwGxnASiHCJKtKhuD3abFPSoNzA2EPff49aWqYhVpKiVHCx8rBJ2OdnUOmYD%2FFOKTBSsFjPPaneW3qVx3gyfpnS3C9Q6ZCMVBR3sBSg3a0LrWkAVuAikwLFbXk92To9gwDBum%2FDFRokqxQtUhU71MIPzn8gGOqUBL3J59GHtzD3RKHsoa8ZtUwKIBipiG19Jd22aXLcZE%2BHb0gJ8byHYQcDou%2BhmecCJdhfZoT4dgjvrheg4J135wq1lgqPZwTXHXIQZYrBZACOkybIo6QhVYZXEto5HX2povB89eTWTtvioOdXSG5yrOBRZsnxETw8inYCGPLBIeOT%2FIffMWdEAIxh1Zl1yxIygZ3Zgw%2FlcLmP52WH7J4bpsrf2Qh2P&X-Amz-Signature=e31040a582eca9dad31fe5b6f4b6e32f2bb6a1ae5bfd152b0e20d20415bc8e2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

