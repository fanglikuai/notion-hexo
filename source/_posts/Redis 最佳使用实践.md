---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2GHS3UM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCyUu4juJ4lx4o3Il4SaoKatuAODWsvxKmNlGWbnMt6rwIgNg2z9Uy1WVs4YdXhyb9Ka9bYeJVpvJQtNuyV46YAKW4q%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDJjWS3vQF8poi3OCHCrcA1n%2BGsQiTBD3rfu1QXLysv%2BxicWb02f6T71sGjYFOz9WY7O2lhacsMHUzR323j%2BBVLCHeixyphaGQoQVUzjw97QGAd0rndrHBwP5mfapkBAmdO%2BlWR%2FOpUm4oI0ZjsnbElkplxNPwfEawkwLn%2B7Cl6%2F1t%2B4G3WsLJLbYZN8YtLNKhdkPVZTF76fKK%2FEjQVK8DNK2cm%2FDuj4Aq1RJv%2BcUuUVtyV1kQMz2DeGIRTIojbkOi1FGLgVrBethTH4PrRDbf8pArnwc0UoothFbZRKuLMUKKlztRhR0ATtV4Eh7oIAwIC3itiSnYlj3mHLCA5sW1Dtxdf%2BZ5750LdkNhPPprxxSkNGSncVysZo51YP12%2FM60eud4xcgMHT%2Fzs2V6e%2FOmoJKRi%2FEKDnvAGQjRwN5jFx%2Bp%2BAwFmY52KTkgJmh8v3%2Begq8HVJu4qw1ljhtEvdKAGgCmsqeTKXrJn1ljKLIl2FBs%2F%2FrBX%2BFkZyPId%2BQVLzH2CLwxUP%2FrcI0Nhlhm4IHti0IGqSWUEoMXpqfsp9DxqQjWbX0peKVY2JwlPlOgCIKk6nL9cpQHddOXe6LxprGSbLONbUdfx4m7ZiHmnbl8jDCm1yWhhxw%2FvJWC6ATbZbHqQYuyNPZthKvIFaUML3sgckGOqUB1ugbwhMc9DsS9VKdtUFoxUjedu%2Bh8UQIDW0eUT2a18r9F5xTV89ClIK91L9awLmMQAvkmQpaUfLgwaXls9HSZXEiSWes5jMqgu%2FNPdoRBEEvdOm1Vo%2Bj2P%2B81aSTO89VDrPScf9p2DjPWA4ztDVA305s%2Fod%2Fc6w0VvWw%2FDY9r33YggGv6LI6bbgXsK9GrEwyVgcr65L28HWPQxBYwEwdbi24rzKC&X-Amz-Signature=856d61334157c67b0a5cdb78ecfe901428dc7b0a4c7c2cae6eb43c8b773e365f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

