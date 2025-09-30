---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SDTQKI3%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQCHdryXGkbwOIhBo6NJxCeJaO71nNPdePMFlkF1eOKIewIgLpgnQmP78tjq5RXqwdJviluXUyQJhh2ToKJYOCbS57cqiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCygbbrESgfW4DxOFircAxSAsP6TVCk7LsXNdRqk16l%2BnZy%2BpgRd7ICP099Ey6qnv6YT%2BpwvGJLANuC0bXm86BiraH3eAb%2Fk3lSFF4AraXBlFwCupBBxL8PvKlrWV%2BKT6tPXaCoAJc5SOFOdmOw%2Bk7H5aR8qZDOzayLNluDwg0GcEs34NfcIxxePlzZwzkgcEfY67IeWtTrgKOWPaNduBP7FV%2FaVYZFGD9E05jFYwrtik8FwTMivUP%2FlGji8Rw5ptQ4xN6GJRqthBItrI5OB7BQMqNCBojpbQUBguJY%2FNlrcKcdEn66YqZgmDgJDKMUAxzav%2FHLB02DpEE1zWsk2ZgsQivOTLuzriKol%2B08eWSTsKKAKf2f3pSLSBI27dpnJcLX3C3EkwO5H%2Bu6MpPwwtZ8wafI46aiPkBfeA5mss5CoeObwIVXwMK6QygSSi2kx5dfAMghHViTpmOfGT1PhHivPwErk%2Bkh3PpuUPm9HcWFliQpYuYMFdNzaojp2JuI22p%2BKFAbgDHrBuQlVnXydTuearl%2BObXnXU4YnJD2lYDDa8Z36MJzEaS3jtcNJJl0dH8ndwDB029kxCHJ1qskfwQcHT62kZU7zmvS%2BhKMrC7DG%2FjTkMY38IuB5mddWzJ%2FC%2FDP%2Bw4xGu9%2FaOeXYMJuy78YGOqUBRe1xmFTEIYFp%2BkScGRAIf86rHk6SW9M3GLaeuMedwCjNoqdCuARBKBvFAB66IjITJxE65j922gD%2FdYsgf%2BHG2NlOOiIbP7UwccVmiz0FqzTx4cWz4s7tnTYTUi3BOQGg6xcRXpndk32jA16OUGxJnnI99W7Vj%2BeqxcstngzjfPyAE1ov2XVpeQF4kM%2BME54672QpgKWQnLZGdnud69ZuO3A3B5Ff&X-Amz-Signature=6be91c47d219cbcd6e5280814782ce0214bf10360752b45501d2b7be006f4c96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

