---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZ4XZDQB%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIBmjsgzq0TILy6yvDRRqZW4Th7FBpEQhW%2Bv%2BUWW3MfW6AiB8pV48kvKT6hW5OYFVYMwqaklW8PxXQ9licKeFOxMYlCqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBwJsHiIuFQ6EM8EcKtwDIHAScLL6kJzkT8fmKZtmOEPGS%2B7abNAk%2B1IkJxC16qCFudfvIbzMvgQrPyFDRN8WrkXr9bxFe2W9rElE8gsoEeInHaM1oO0cvpRQOemKtf9wzigc0RMiBqAiF6SvxB25Nuuvc3t%2B3DUIwmklt3BZGe48GaEV11R9%2Fg4P7dDEJQp%2Brt0NGwZDPpp8ihNNfhIW5tsbJ6jZQwfG9%2Bj88%2FO4BuCsH6lqVyq3UbE2Ebh%2B4A9P0Jl4iK%2BUIo2tkQm%2BF1H3wLxxTewcg1koUBYG8t%2F8W1mcJqfCu3L5RN2HvOciBpXOkTNBR%2BWpp%2FPW5vlonw5257ohLC68ldkrfmWIpMlCA%2FN23VxEDzye34t4A8VXV6Mxyc%2FQSnRXX9L4kWsbnRajHDZMy%2BuNpgwM4kFhUW9thvKWfmAQ7Sq6619tAaEO9P%2FkrYH6IpqfuMgO7Q5qGywdGWNOd6E1EKioLngBKFZefHNQ%2BiI%2BmMaa%2BdLjiPx4yvTrteNWw1F7MbIjzVeYNUjg3E%2BY14V1Nw38y6bjLYG3Rl%2B7eFZTp8Ke5mvNRNjTI15P7WDMPyd7271LwkjAuNmKLnjAtK%2Fis6WlxD8ZE1ebE17Te%2BzBvY2LZM%2FvHhNTGqiu%2Fcv0Ur7qk2TQV00w6%2BipxgY6pgFLO0JfSRoxd6HxaoAusMIy9aVo2%2BiBxjLohuxQwSLJPOxuyinQTioZZ4HNIbI7OhZtZ%2BUoZ%2FgPQ83asAL85EN2mrkABwmkocLNGTbyt%2FDUEQonE35HPwpHUeO5NGm95NEA5eOPYAR5foweneLUWJBqqzDWGlHQnQkA7Tc84LyIv%2BPnrxevTnYcMxCJaxSJorqI9VZ49U%2FBNJ4HdF1CFzDj11%2FWqL1q&X-Amz-Signature=1b7e6ed0902680a9d6a4673e180f005f130ee9480832a83cfc2f14b6cb1d165a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

