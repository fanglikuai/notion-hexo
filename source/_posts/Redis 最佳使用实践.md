---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTPFGKMA%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG4DPatU9%2F7xa6Bu893p%2B2nc0igsCJLSgj1LIVvTIrAmAiEA%2Brlymp8YihmKJ6SQ%2FzwhIujG%2Bj3SAOGXSdg5zi3MdgUq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDBhpXFNab8JcfUGiFSrcA7wy1heiiR%2BzyGZDqsCBaxCtGtVS%2FZYw44PbBEidCPzBNtXcmuOHU3jz33VmdsoOmQcaUFUJ2VFAnsS2SuS5Hg8sSSGLiSTWXjRfycXGRuckde%2BN5S%2Fkqo%2Fr15in8nCVJEezzMKPxBZQuk9v%2B7qIIMpbgCPBws5dVOfwtYuHGfM5V5sttwiCGiuXd%2BSXmuDjGbJQCmNZE2%2BBPkIAq2QN0f3uDqwp8dxUAc3RQbPXX5cZSJLLpQmjfqxJDAhKOuXt1p1H2Jwqv0UdQbjVQ%2F75p8IBN2X%2BYpVSstCJkXJ2tpQ8xT%2FOIkVcgzDSxMp2j%2FlDDxtehCQJjCP2ayRReTl3XBev%2F4t3h2zJ%2FH1gu28SwS%2FNNY65%2FmwDEpEgyeMhL61sZ04JMoDG7kaX7bdqZ5r5LTFEMXhA%2FB0c65XZ09puvCNRO%2B5YRWoW3nJ6LwxBS3um6W3dCpb%2BMwjzkBGcRI9jX6TMrukfoMkQF%2BAEQBzN2Omis6Ica4kjU3FqpI5OkdRIZ2E57aAqmQ%2F5a%2B1MNK%2B4io8vRxCitZU%2BPdzTDK7E1MBhiec2rGljzJJAOt4Q7eCJFMn%2B1BYscsRlscPn%2BN1a0VQXwBx6hCaLVzVA2GKKhR607qlq8wyCqG75El7cMI3d0MYGOqUB5kQfaaG7Nk4ULFhhip%2FhnbTsDu1D%2FoTFFFrSAJ%2FU%2F7CEcRT%2FBuYIvfVxyt0GTwvUhe1zQrwCemY%2F2ewG1lr6tOmJGHJ%2FcWy3%2FuiItwjIeSoZdAF9SY5yVlp37AuejVQ33XCmbNseahfdG6Yq7uz9lQ%2FOkzYQjaAXLou1Va4j27uMUL2OjSbUNeMpHVxMMpZP%2FrvNTSa0eHwcB9pAlRfDXVhlTY7a&X-Amz-Signature=340eabbdf09879d1985647553ba61657094d1d68faab98304f1999adcf7e7fca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

