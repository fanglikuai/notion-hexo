---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666POEOM7O%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIQDKNEkm9GK1qsxPH9%2BKxyQ8kDHq%2FML0sJAVpqsQpCXvrgIgegHkKp27j%2FI%2FF6VXa3W9bZowIkn4UJaQAqI96f3IVmgqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BUblcoi2EMio7P2ircA0pf%2FbCl6oQbfSHbMz9t8GsR9RaeEO3HBUgHwSw1HD70pXbf5VYJqkRk1TDzIA8KTQLPAvCalN%2FIrm7l1XhAzZS1nJHVECb4Zg1gmJ741f4yMm8siIhvKMZgjAWESEM3yMZTZuqHRhgias9IZ3yRdkYwAmysxAdaNsVhMPeD7XljVE3Vau4a2b6OzHA8KblJyjcBECUb%2BtbqZGK5nKZUDmzdXuVWMQu%2BShbEoFl3EhMb9mTj30PEXmMcMRcX7fKDtytlxKM1qONQz%2BTTp12oiDT60DX7ceNr6hRIFPUuv58iLFg9%2FoH%2BaHeqzOszWxAxKUOVJ2dof%2FZqc2XeCGVMouINMvZRq3LHdl4ENyD%2FMGI55WcpiUkTkMkraACuliHlBGXK%2B88jPd0J3AMmP8hHqReuSf%2B2pJRkrZZO4jp05HZjmC8KjhTEDM1ziQ22iMtBclU%2FB0E%2FJi7vz9LY1yM7yjC9czelmAieCG9l2qMbIX0PcmvK1xOAlI7omWh1oYy7Q9LJ0LaErh%2BxuUHCNVevMgRg9NcKkC2it10GwSnlFPtD119vsR5lcPEfO6xnAD5V%2BfE8RqPSfZSieS0bOVMLq2Y6HOX2N%2BcDjdJCv1t0r6FmmFx3L%2F9D7Xsy8kPZMOqp7MYGOqUB0OGizoTGC35VdOnnw66an2ZfaNV6INBQKrEZWLc%2FRnL7XQQSTE4HAIhrb%2FCwZKerKfo6Rxq1wA84nU%2BWvQ10vj2EIS5RvZWr4s%2BPknPOcSMU23VbcNjRuIanyDc56twv%2FJec0HycojQQt%2F3hJUXWw3Uhb3zQCOYSwhJF5RqZQuDoHYRXGnpUBANgq0k%2BuNaDXERC7DVj%2BgU5sXge12p7SCbqlP3P&X-Amz-Signature=fe900af49cc2272ceb952fa2d8aae1e6cba313fa78c0946d1d0b7d4511eaf75a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

