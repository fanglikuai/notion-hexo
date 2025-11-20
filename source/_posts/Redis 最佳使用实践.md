---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664Z4O6DGS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIFQpleJ7nVFFm7JgkVHXvc%2BtLUJtB9xHT4crvHiydJ1UAiEAtFoBammZzzj94%2BOsrN0RYv2GfmD03Zk6o%2FnHl1TVs6UqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcydLyCfsH5oZbpJSrcA23CPKW5WIsEWTK%2BYfmk6yg%2FWV4oG36bNWjJfLiAfGIMCnCpdu2bWGrBpRtoNgNpSEKczR%2Fj37EEUeSMWSnwI86Fus9Sm8yidG1%2BPojNaELfRHberjqP400F13crGvILFPeTQ6nTUn%2FZbcvgPWLB9vZvUKCaBiTjsk4TQ0Pff%2FyVEVJqcCFp%2BKxPohW0Rh8e7%2Fy5knsX88wcNpZkiMseKRaEtdHV%2FSYxTsPP6Kj0bEL1nMmTQiD%2F9pgVZ3JuRbJ90Zl5jfhDkuSEuwW9MOwKpSyKQpIeMCo9g6DY7D%2F2qldrC7skmULvXruvRr4tBtEnYeInP7g%2BstTfqRaole%2BsDqaiEKvICdtpG6xHmrDm0qZC0rOSpArmZ37OD%2FeS4%2Bzl%2BbU3llq%2FMwqdRo2Zur7Go34yYZPqNbBzx6h7ZUb12CN%2F1T9zz1wUneyKyxvdO7yAMAee%2ByVmAhMySDI97wbVOdLuq%2BgOX3QDpS4qzxDW3vTpEUa5K1tmrV1wza3VpnZQi1mE9Kkw5sdxMB7rVyYI5iFdi%2Fq5dfjxo6grnbgOEXZf7ma4c4Qug1D08kAKSmNnJiUXYCTLuVnXxojZZ61m5QEPnb%2BrXUANkBybMchSqj%2BxyeS%2Fewe1KXVxsDseMImK%2FMgGOqUBHqN8gG9aWSLwG0ugdBg1Rp6lRFoKEjQVirYlRN%2FjGjjgUqvPUwcdy%2BO51o5VVmFWylAbmyrznas8JxhNKk8VMr20oIar%2FpzHLQJixq3bZMGMEIqeUGK6rMFKoX81xKKnGTVo9Ub51At8ezkzgAShgexI%2FS70WFgS0ZhiymmNxNKcdGJ%2F0hZObhdo4%2BeVy8X4VA6GRl1ors8ayDLDAiCTVmvdGM03&X-Amz-Signature=69ba0eb3627ddcfbb48c3cec1ca4827b48e6fc7462fc8dad4f8cc8bcf4d14365&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

