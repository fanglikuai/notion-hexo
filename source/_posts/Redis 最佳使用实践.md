---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WA5YRE5A%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIDOtNVtRE5MsqNjr0zYFZUqAahyPvY2fszxZxLd4YVC2AiEAovJArzN3BvtIizu1tVUHdARcGun5Jpe1QKzTx2p1%2BmcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtOHz%2BkVjjfFZrjZircA%2FEqEgatkJZnYGZ%2FT7rOtKD%2F578M%2F1%2FQVULUC26gP1fUciBHuV3RL39%2F4diiFOewpv4FfIU%2F9%2B3TTVrFgUJY%2B%2BFhjoLF8hDdCsLvgPJzzGKtkdNjaMHS53hY8P4z5TGk8Fm1yt8AGyZQ6ebqX55I9r9FvMzY2NbRqSpAEGS8JgZHub8FOy9AWRNOCZd05BGcELXDNlMz7EjLReZ4esiUxhwXI%2Bx4c5EcrCL577OALCxNUMCsI1AWPCEltYbUxo09d9J%2BC0bkUSOnuMgWMKUKLzcecX47qo6cWIfkV32z%2BZpA93K4yHtuLWeJDhjKqGZVPGeDOC1n7J%2BgtpkhmSI6qlvq76q85EPc7ny4R8ePigp0aR6GESVWxUeLzhtEjP6UpmMj%2BaUCKn8q%2F%2FoVO73Cgu3%2FtqTMTP9SoUitADkGCjUy7HPRaNl7cqNxbvEuC8JZB0NDPaKMqA61%2BLEBfUXdO3Su3mNev1PLlTI9z6jM0MvR9QPhEeOymQgfAWPU3ZQa49eUahxHn9Q%2FqB68fmqBTgWP0LEHj%2FHMbbKELL0McU2f3qIJ8NS8aGYSQF3sqMHZW8P00qMHWSRp%2B0HcUfROwseSQ%2Fooi9f26jujpKuSgFugkcGsnSTxlrqS0IKeMMrlzMcGOqUBC%2F%2FlDAq1DezLmMfljhD4DE82lAp0HHykHqwFly1ZBI3Bwpx1ElO2UAzykd02%2FxsDcd9xrM0dKMlXxq%2Benf4ma1Pmn7xToicBlhyUCCUuV%2BWV9lv6KL0uclp9bSJQOtIDg2tMedNUwuw3RgDnmiD3GYfT4Y3t04yswzuw%2Bc9dk9v%2BOnq9Wn0IC037TA2967rAvh2O%2BH9Dt7LuvjOe4St76WBVhZMA&X-Amz-Signature=4e6de2054f98800117f1545b2a201b368084781bf33f0c9fa036419c59bd96a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

