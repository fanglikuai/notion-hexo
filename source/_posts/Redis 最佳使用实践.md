---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHBFO3TW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T190230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUJytxeoPOOuGI9vEeWbuLEMK5W7mEey4E0WbO589GsAIhAPO6M1zIoF4kPKs8YUFXuTxQPO1y9qss07JvV6o4LzspKv8DCHsQABoMNjM3NDIzMTgzODA1IgwmSB9oMKq9mfvAjHIq3AO20qJj1Yi27IlDrmFYJ1pVaOhtSrdE9F%2Biqwh%2Fz6J5ZxWXm%2FB%2FRF58nYrNV%2BXbS3vbw6RwNBBG2tle0YjlTduRQcVuqu3rlXItS6WwinUmNdxNnw3UwtZd2%2FbqZsgCrYkFTK0X18vXMTTMwIhOZxZeesatqD8gz0W15YdM9AmuQ0U6Fav1POG3fe2n5EIBPPFiK4%2FCq%2B01L%2FWFrVkBA%2FNQX3ThoqodaCRLDZNZdJz5BOUj649bK4BtbjprHFjOiiyUplJUQG393Df0JmckllOKJuq%2F6lfG1KdXXErnM4b9%2B6gzaMi6ZAuDB2Dwe36e%2BW%2BpsDU7a9ZeZfL17XELeAEujow98g65kD35GV15reLM9m6a7mScolNemPJal9kXpVpp8WVehecLGzwrFH%2F7uEQ0yyqYT1verOJuXmpPsoy14TD7AxSqVVpUShvOlVYU%2Fmvi0kXR7qnecc5f1CfIpsW42VeKymAD8NWpGMPGr6wB6WeKrTRiSsA8t44j0OfdxeNAA0FeJ7ue5lYwlkaMbHHnnHYygtBmcnE2qzCoYPLEPbL8D8rlJwxfiLXUXHSiy7LbVk7I9QSotjPdkNyUDrt4YeDLsXycpS3kjiKJCXuzYDmuQyArt7cqqzTzFjDYyr%2FHBjqkATDkrOQokpT6D5L4i8zLE1zx1c8e9KgvOSyL%2Fe8nZ4eBhcd9pLHN%2FNP5Ef%2BWk60PR3YhlUrvVLSZH%2FJGWYLC1ySg%2BID1Skf2izpt8GNq2XoBKFR776W0nY%2B7rfrO7eOKcPXcArM5rnzDySUbzKUtro0g7rxAgprsEBYJzzJaBIOH01aoYpGkFSxEvKBvH8ftpCOYg3ymCVuSsp3gFyKLwASD1hJt&X-Amz-Signature=95040a68b71e7fe12fef056863d0a19f030afd2a310c9a881c877eaf466343ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

