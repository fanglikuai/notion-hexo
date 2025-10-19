---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T4KLVXD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJIMEYCIQCFYsARMgoPgFt9D9IoX7EWzDzlUKTzG1afi0G%2FMmag%2FwIhAOWDhub73RS0iXc7bJGdA6sOMh%2FcIeioML%2F6zun033ouKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzbGhpGP84Bc6KIYPIq3APwrT%2F1rc5bnDSKdrEVj9cvugrqyoiI3x9pVczjQ3ILhbOJXFgpKkX4ozxNauBER4JxW3ar3NKlLuP28CYIot3V%2FOp3rD0uHCoCWQjz6hVUrUeqdIdVQDE55jwnQcZavgdoVwrBcZMRXxdh0JMnnbxH2oaSjKqvt88oW0TEdAg%2Bw1NJt7LMR4XQQhW51T8Wxx9fEji6LE094GqS73gRo1W6TUuXFdf%2FghLM1m7jJJMTBIotl57TclWSEVOgeA0L632gh4oC%2BaenttURwoW83cuEp21MVcMX1v6AX%2Bp%2FBCE9lfjvXo0dzNW8SAdA78kXFNwJRqNW%2FoBNI4PupzRiY4M9AcyM1n11cdW8L5xdF7g9KL%2BKXJnk6lp%2F1jNFLupWWMk5eVJN%2FiNb54ezijt%2BFHmZ9AlifvUarr7qdjP%2B7H3iAiwQ4eg1TBmHFRLdh4O%2BSU%2B13LuRdHx2WPBXs%2Fi8Us4Z97YhzxHzz%2BCHhiYMpZA5%2F57FlYeCwF%2Bcuq80atc2onpEurghBnNZtBf%2FgY5XCy5gz3QbcSMjmSZu3%2FNIfqWS1hov0guZmfrn%2BOdmE5XPjFwn7XK5FBrcyPXfuI4Avm9z6%2F3Kez39%2F8Z9N7KyEZHAgrYqxC1N4qp0bHUw3DC6jNLHBjqkAUN5vyLVqqK0BfaXKNwroIsBJJLxeqqYibGshdxEcXj3kj1GSyVxdlZ8BD8tBrEIrRO7LqvsFxwHq1QBFUzjX2EFcLObeLrO9sTnbTTWKIyCEn3i4ImWBfLw9utgWLAb%2BFtQNcOoKsHfjMC3T1%2FBVB5bW%2F86SwhRd7Rrm1ihPO385lgU9FeCzjt7fcXf2TJ%2F2sMo6j0P5fkFnokyG9FAHHbAH5JV&X-Amz-Signature=95525d35dfd1f2c928a231e88826b191634f8de2a925424a29f1c8d1f09dfb70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

