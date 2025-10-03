---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGZWINKJ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFyLwsgm4GfkEyfsJ8UYpmp1EL8KaqgrzwD82znUIsVvAiEAxL92iFaCe6%2FXVJ7mIWgaq4VPgvPeJJHXoGUye6SkEjQq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDJyW9wJtht50pzWMBSrcA1G9FRG7dMPcROEFl02NU09tNuWa5rj7Bwkf5Ke5QRAiY83Z7gccg4XJkF0aFefA341CH0kHzxv%2FKgK8F%2BJ%2BrdqC%2BGnlIphd2uLxAgxcVj4kAO9ll1ZBR3aiC3ipTsamAbHo7yHEt4wWM0tQS7QwdlBif98RNYHGMzGQDy6aKWCqqbFpAo2EOZOrDl2A%2F5luc1uwuxII3LJMFj6PF8DZt4AKwX3sInUQMy2vXeqIIsoy9jhuV07f5nMyN3jpsVWa3L%2FkR7vQCwVdhf1fQkTbWIaXvwk8toF0DFhXlCT%2BFdtQTnSDF7mrZ%2BD0KQt2n6qnXWmbbkYNKc3S8rPEjGcqYHUDOEFo9S2AaojbepEcPag8AfcjamCxkXiT8mIDSVCyX1Hwq56atDvCAituDfs%2BKPuh5hHA8qezsq8ZMmZzBtX0HpyxgaPK0EvM1P4cBbEVHo%2Fmybal3Av25DCnYFhx3YVZUN6GKo8DofIyUWqj0cy7wD4ckuPWnY%2BccmIMFl6uf6nmdr2%2FRmMkKcEuzSjXACEmaIUh6Z27V3VAox%2FidMYpWX6yL8XMR%2FVEeR%2Fp8D3NEZ1gL0xzacqGtzecv%2FUfI3%2B6vLoFZdKkNT8bm8Ht96wmjDZRnQPJ6%2FjUFI9GMPn1%2F8YGOqUBnMrKEQF%2B%2F83H89WFliHgjDX%2B105xSWgeOblHTZear8B3x%2BcRXd8A%2BOFad9AaoclhKG5nEvKx4svcYTfcxgdzLoDkK6qkvRzyqhpb%2F1oKqXlEUMwgkZmRbnZ8wPpsQnugVSLLhcSX7vXBN%2F%2FTZqQosEqfbIvuBxv1Uvv2d1KUZVxWXZSIsZkEJkSQtjnHwm8IYS3QEcgpJtSxcxwrM%2BdH9jONjOni&X-Amz-Signature=9b9723593e9bef13d939771d93eb22132a6815f139499b800973a38bce20d5f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

