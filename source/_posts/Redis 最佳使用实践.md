---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356C2PNT%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T140038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKIyldNZBg1aiFgQOjacE%2FBNi7k5sZoqp55SNHfVGI%2FgIhAInSvSklLNLketZEoXYVeyFsywUDYDLO0raBjQBV1nPuKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxnmLofKHmFsTh8Hr4q3APGjlDGdjDZPsChpa16A4iRE5Q6x4Lr%2BKzDJS%2BbETFP5yjceSqXfXhticG7VBlgN%2B%2B4h3q%2Fk1ERb6fip7aUei0zX7Fl4B6b%2Bzuhi6SeWBcRQzY65rGlfzL9z8IccYi%2F1NUmdR%2BVPGBo%2FBZJYzItJ1mX923y7kcTq7Kvg%2Bz6l%2BzHzk81IYqpgWBP%2B9Z5%2F0HVZvJayRex4yw35yyH2FgEN4RFoMRmUGLO3L8gYe50L0xaWlVxb1AWURio%2FrsJ00UmfSHEZ23L8auAMyAWpC0ioZEQsWXjvdvSSGY1m0awdc5yGTb1riVjG1t7EPgXcKuNaFgiUIVZEFSRirmtHuVdZM04qtJ0MhZ3Q2ci3yOioB9ROSW9ZButaUl9rxsH8Ai4g02xver9wDrWVAIE9YUHR%2FTzXyz55NzMyoce45BXgJ7zqCXxNgfXJ3xezrjqIJOhHkBPryGpF73cwM%2Be9X5bcz%2Fj7bepq3re0yCkJOEDje9aCHMeRiGYAZUMSCw1uN5kx69iPZqAlObfm8Y7rOwuUC4FJYatMrr93oLGUTWQZ5zQqahjTlGSvc20j0qTdc3%2BpyENAuRePifRs9p%2FOK7xQbSUEhCHw0HlyPKE1gj0W6C5IG2bYZbTuokNzDH8TDC56cPHBjqkAYW5OKWO8ltlFru7UomSuoVuTaiRdLs7clunjHBROIQd5eELOs67HTnqgRPAiJ0IN2UOIWGR8TgW6lkeMggLBSXxRSDPnMRY%2F1L5hesXQElb%2BLf%2FiIqMO2SRQ427P89ZXy%2FU48xdCSZLTUz1f2EMtRviOmggYJ635%2FYzPdLjY5hNUHRDSshbhVi3zMAivA4Xyy45KfMUeAORP40QocW%2FgUw8gXno&X-Amz-Signature=488ce89afc69758a3d6be138ac02565c4b780b876bbc0feb3841005491087cfc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

