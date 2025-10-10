---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WIGTJBY%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQCJH8xTjt%2FR0DqKW1iBOH6f8p4VrGDXiSBor0tr2bUOpQIhAObNCB3fZkPA4PcAAwVJdpABGFIBPTVUNvwZ%2FQDN0qyRKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzVcpxkMXJeWnuImT8q3APTtZF3wfNbfyJsezc9Z5JYORXrmA2RL%2BJxEQtjj20DfRa%2Bu%2BvSxV29WCFVt%2F8uaG1PP5bTk8d8Yt8wTUwwaK6Vdc8IU8T5wJYG7NimUdnA7a%2F75b1MSjfF5syneh9zsNkO4OQDy0XXGbAxeH%2FfTF29rxEMxrgMhPx8nhsjC76lpGFXzts6ZUcP2x%2FkpMpcbqlv%2FcbuZ%2BdW%2BwuxdexIMNTG39S0ugTg3bdjB2HQM8KUDUqoDgZgHJhamTXtnMvdh2nd%2BFNr7pE2MKDmCi6L1jIraTEP00DW7oHPP7wi%2F14Ul88y3Xpq7HCTj1cb7nGD%2FLWcNGjDoypngkwFY1JDaMvuexEDKpELkhZhOP9%2B0do2a7XtQXyl7oI%2FVTajNXqcr5pr4d%2BWn%2BE%2FKsy5L%2Bl5o%2BWtY7AaYGCzWKw%2FyIU%2B9GA6JCxs9jb7gf%2BbCJVj1CLJ2o4owFYVTgQVKKGQkphS3vxC877MuYaU%2FROd3sK8r8zI9egQMK%2F85z3YVanqOu4ZVfVrG0bAvDvrr%2FiRoYj676hSg2id5CxWwV1I8OOORxkG0IQv%2FY11qT8%2BIuE%2Br5hOjT0IFROU0tW3riagGIf%2BDA0%2FIdKwqkXpRxFDl0TJM8vQrYrIFLiql0trJ%2F2LDTDf36XHBjqkAWhEGmuTkX2jYsKqGyrFC3dW135uC5LfJ9%2FcGBU2Lm8QXykk5tPCrOC2BRnL5umT2hN%2BUt7GCGM4lD5q9kLY1A9jAVVICkRMpHyYh%2FLaaS1JJBrSx5T%2BQAA8rWZL8%2FRFkYRrLdTvHz1Knljn%2FS07Uz1yPhtQ66FjSJQBocSswILsubuiifkpFMlFxlycAfpf40iWshBh%2By7e2NeEaR19%2BMfoNVDe&X-Amz-Signature=e6dd7eef39bd2ca7587c09963756cf83f991f5f733a4f444f95fc3419bdc61a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

