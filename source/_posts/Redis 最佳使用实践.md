---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMATCXBR%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BJ3Vsfe4fdL%2BiJmWTV%2B38qERDO936SVuSkDkmyTxp0QIhAOqqHuSJ6udflUeiFOrqqBqbCoc64ZBVDIxJrZYR9n4bKv8DCHQQABoMNjM3NDIzMTgzODA1Igx%2Bd9yroh4qOuD3%2FcAq3AMus6jNpPDU%2BuEHZ0NGOoRLax1aTGhOT7r%2F6AF4%2FvtOClJf%2FFEnel45Qun2JvAnDivV%2F2EaNQDNFobEf7O%2FwkfESBoRaAw8qU8EC6Js1zU1hkDA2jaRnHcLL0sxWeD4bf5DR8VYsmDLmdHgbAMuiYkwXd6xmJR1xfCSbR2SAa9LvMaGvxM9i7g4qeEz8cbK0vv9XRWxyoGZY1ixqRvrzoMwWsPw7W8s3ALymw3R0eEbR04h4o3K1G6oQivqDSZKJlMVEKAq%2BjMJmD1PAH58S6MP0qCfryWbgulYxdgsLEFKUy38pDCCVRPQXou5nM02anXFn1jHCtaGYyp8UVY8%2F4WmUsOwIe9aoX%2BMlIoc87Lr%2Bmkp1BxGad9Kel4Q18i3wHQlNJV61XXW%2BArAXKklOr6BOAyFm4TxUqx%2BhbWdMXFp9JNqC46IB1dF17zVXxWrYzjMeffLRdvsdLI8E6qckbQ5F7jK%2FXKoE8AMoHwAhH8kijTaphXO5qBfsQdtbU2nXR6AqeFVE%2FAGHJeXt5zyDH9RYnk9aGqDgSuIkppDX3CnCLPHJI5Jphv3xvgPiibwiZHYT4wEvlwVF5h4AiCYRGPMFhc%2BgIpkEsllqOZ7lDCYBeu0Bb8YJCzL884t%2FzDA1%2FLHBjqkAeuQCx4X8dOvLl4kE4223CbIIqUPSCDmkBiMfPiqObknoE4qXOeyR3d4M%2F6%2FfX8Rh8Zk20liB74g2M7Kx78cDVRIt2ZZxPaVB3hAuAUcOU3rwaZA3ANIi2FYrXTC4x2%2FlVxadday1h9i1VrkRgtLmJafAdT0ZbN64RUni4ElWMxLDyvws52%2BqQq7D6wSWJUxIl3R5XZlggpk9UEv0EHdEaJtO4X1&X-Amz-Signature=c8b1dd0e3ca16a760d5d7dbe0f9d497a0216726add503a76d3ebc150bdcda0fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

