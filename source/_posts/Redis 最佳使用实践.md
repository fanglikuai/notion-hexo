---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRCF3OZ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICe0Q%2BvunMWMN9L19MZn9kAlFZzNKRzEZ9R3zAiZ5lsgAiBWyeA1zEeQcK%2B5BdYjCPBNJmRSU89nZxqc8S08WhogZCqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9eawOO4jKwSxb2JQKtwD6dhK4IIdI7ODs08GPxlP04M3D9uBSysrZBc41ktpbHoAmAn0Pf490hd3MoO30Oc2JHl%2F8ZjU1HYp2LqCy2aPwXlUPWZN3bg90hn0xsTLBdLTc6UjETJl7cA25Dut1WT3H97%2B%2BuYHTrfKXYckLtvP%2FdyNpV5YjmLgfoto%2Bob8FvMaOMffG9AC5Ceefz11eJ4Kx%2F1TharPOYDmAV3Dt4TPV2hRvoDtLvd0WUE%2Fnaw95YdeB1aoDw8P7rd8Boa%2Bpua65MP1SYrsXRUd6eV8aKrxt4qN75S6kS3%2BPkMD6vh%2Bvrt6Vkw5clw8Zd5kIrPThxMx5DrQOyZe0DSmfXWm55rxjlx1NK4u6w8Fj3hOL4UomK3JEjbcegVLgOBuJBx58CD2uuRJsW0RgLBjFDScMLzuhcL6BegZ5hmWpFwQpYMiWnPCBEm2K%2BwKFpFgdWs6HuW3T6yRKOpspmeKvlHU15SW6hyUrkucietz%2FwwaeHvMx08QNStNXd17wRJzUZFgix6OmA7wEqQ%2Fna018BxcdCi3O4yqHLnqv8Q8qvi%2FbtlOpMMOjFz%2B%2F2oZaOoeXF341yetvvJNyDTUhOp58IkZeMvmSa693xlIA%2B9AE5CJ7C8tHr%2BUnMIzWCieJISPPo8w5JuJyAY6pgEh81cE6bhX1QgKaCQZljVm%2BHFZNjcvoM%2B2WQ%2BNUi1OesTN067MthV2R8OtpsjftC7LJV1q7dTkltzSvlvWEXfz%2FQvu9XGC3bCGTDvNBliXVIWUr0xqUsu6kIS%2FmYsbMr2G4ppRHPubHz7LduXiWBSwLC5l7fpCwo8TKkbktzuduSKHLnP3pm6%2FwQlI0SwixM0fE7hQUBFGZxwrB4wpgquYpF%2F5yBmt&X-Amz-Signature=7ef25372988d91482ae5df764978bc33e6aec17d6286c964fe7b231c1476dfe3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

