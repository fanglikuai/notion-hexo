---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCWYNWCQ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1c%2BF70vQRD%2FUv2zDNTM8%2F3NdmP4ir3IPeA4%2BHsue8tAiEA5FAf0W0%2BtV64Q0ed%2FXiaRONC7Vk%2Bg%2B1WYNXhD9u%2F4sQq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDHk312%2BfFJRUkXYlgircA6jAGmEEUhgSKCnkfxGSj2XGx5a0LMRD82qlrNDhh2sh0HJGqDh4rXlvLFkseRCITp3VlBZeor51nyyIChMamYuXq69D8Lb0vYT5qg6SC05R0%2FkpVhBdAi3fiXiks3npz%2B0uFbAlMP246nNEVefn0j%2FbA5th6F1RT09fdo6Xx08s2Igw4WlodjLMA2zCkcvS%2FKMmyiO5Nn4o8J7E2AGGJStAIGWAPBUFZUhafCxSrbJOomiwzo69Jxip7c6ZpbtZ3UQ62%2F5zBvUYd2FrYNloiiLHvAoVxM%2FIS36cRvt9kDoQn4l3ULCnygUPKkMiIK5KXSmHYgjdjoJwl9pG2ZvDGR0SspaHPD1cFxBTEu9Pmosy2HjtdeRS3WM6Ulgo5L16dGAKvczsOKI%2BrdzKVa1bxhezY66tNRiuvqhPxQ7ZIKWof%2BD9CMvcIvYgVNFtnRn4Fl4Gq2GpDu%2BwXOcbP8R16A3eZRTxzPkGuNhG8nJ%2FOkx0Zx6hF0oAzFoAwNQAWc78xPAC3%2FieU7FL2%2BuXg7a7%2BXV1KH32r4NAuy0s87imEbFF98eaOZ4665JDJpy0DLwijZZXVwJiye0h2BdZiVFplN9cdzlvgqfnOGVGyFXAcPFBFEL2NhLQ325hqyQbMICQwMcGOqUBJBrEG32whBV5xPCNjlEaHJHsFLXCRN1SctfmDgJmcSv%2BnT2ZH39xAvgOHH96RChgoOI8pPbLtbDXQKhViDKvegZV9QqtTgHXX%2Frvgje40%2BUdDylhXyLOgXxOtppcsyHTAWgRw0e7DuTjBPts4zG4855ebyuNt09Us7irIxM%2B1OfTcVkryH2XxESPEma2LHHPEyx3ui5v%2F0bESgnQL%2Fxh%2FjFjLUGz&X-Amz-Signature=8456ab9997f391dd9379c3fe8f9e30688a30871a1ad24b8de0a4e89801e8fdf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

