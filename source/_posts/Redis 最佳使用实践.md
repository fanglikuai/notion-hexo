---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPMRYXDL%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCsbDj1Si5uiM9XDe0klFrH3c5q6XLLgh0%2BIhNeYnNRPAIgLNLnzc0sL4tUASPYauI3OM6%2FpwC9PfQq1mRFUUa%2F3n4qiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3VGnHB4px8jRe7ryrcAxWCK58Q13rOl5f0j4V1pELGJuZgDfA0SaBxEhjWMsaZZ1fpW%2F8j7ClOpDoFqeG3FhCrxx0A%2FkzY%2FhOifyuqFRH2MxSVl32jVm00L1M%2FJoi52zXrGyB%2BIMJU3kcPepCmrlraAa13vIGHTyl%2FPS%2BJ%2FCSwXwwkADEX6ZcIIl%2BGnusrjrTr%2B%2FlowR0w3X5DWonY5HSEcfN6oc4q8AA7uIdcXjyRtG8%2FkHIffERIdD3ZnLnFvJkumiruW7%2F78RPV23A69b%2BUrbHQIUiCJuLK35aVXoSCa2OD9miO%2B31S5ww65NTC%2BQgd8u%2FseAKa2wiCRku%2BybQntR207vJu4MnJKxlUuFP6uOYW%2FyuHMVMmKEJpoogvGleXxWD6zlmRsLv8uWJv7e3xuualaPArStm1j1rQp6TLqbzltX7Ac0kgtMY%2FqbbiZUrJgTKrXy%2FHyUY7fWQctQ329rrWGyBcWGWaMg5JMM3tcrUigI7eobAi7UARL7NBT8bSIFMQxA8QMxP4fiA2QUr2vQkCo7JL0z1uHQr1r%2BmzikWgjLmxyr3QqstUJ%2FoaDXzhuDtpqXimQlTdCYaAeVvP0i8K%2FCqsRTUoJ5hq0CdaujWztZh5MJ0Hn9%2Bq7ZxBXfkWfoPN99AVS2rnMPLR%2B8gGOqUBBLnHVNC7xVOJn66URvZujQJORI5R8Havmf%2BCxHDG4C%2FfUtRQ3NxkwEDpm0Fj%2BVj57SNLYvBozN3ll9b4LdFW3hnGVDocXb3EUogQuoS7RsA9L8HBWug%2BOUxDhhY8zJFjVsOhbD0wy59B1hdHgRD3dStVG2hdoS9BrTMxlOPqaCM70KUhn%2F0yk5oxR94G1wF8XkP6d8%2BUwTrLBJ1bRc1TFIt2qyrq&X-Amz-Signature=494ed5a89510924e983c6630def36b9399122fc4f9242a0b016f1e97afbdc19b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

