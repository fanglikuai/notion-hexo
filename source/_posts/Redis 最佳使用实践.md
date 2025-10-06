---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBZ7TWHQ%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAiQ%2FLy7uUU4wpI1EQewzI%2BdbAs8gwBX1wMVWQbiB2rqAiEAnIpSH5NjEZ3h9x4mziPSpsOBgU4N4f7gv76kpUBXH2UqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLGeS4ZqCI1FEqIkYircAycOWIjy895sIUykMQ8sd4VmyA2ysL5ab8bJLT6OpNPnSk4Dt%2FQiKNaNcu0XIaPb4pfhD4oLge6CrD%2Fr2ZI0IvWPyBnLgvB4z7i8fcTVkRO%2FKUqOdmzmAzsLWRE3qRozjEb2CHJdDqaRJtOGQqTzwJ07o2MWasoyJezbsd1moVbGCzg3%2F5IrttTnqHbEFVUqT%2FUmGo%2FCbHqkbcViAZB4Ka5iokVkkU0vVI0e6aDbCPpHvVFWNTkbSXUpDBG%2BLtpu7XPOk2uL%2BKQQxnrsSk6O62M8eRcY1zyy4UkxNepP3LtB0L9V5rR3Rm3G3Za%2Ft9VqfFMM7Zu96xW5Mh5h3DDMPvr4IWHRgm16B4YksUclwbfaOkKGaHAjETiraRfg58hrY%2F6SgwUJFQffOEpGBia3lgS8brRjAspls7jfzJqpkvWegrTXxbvBpOFQKIdNQB%2FeOyjS04BVVjJLJfevQHQwZKCeYCwo1MQpdNsklxc61dv9rnQslcqLcvEEY6Ax%2FvxBcw9NwyowwzJfw0UaDk7za3XLTPuggm7g4%2BMNbDjn%2FO33H2UXABZkN9AkhEroPx%2F1dxU%2BSHtvJW7iXAnIpLt9Ddxw3%2B%2B%2B0A%2Funq99J3Phf%2BeiYXSV8MubnrxrkcLFMM3%2Bi8cGOqUB1jX8s7RUlKKZrd3yo5WYbWvkci9sk%2FnwONijJw88lbnkFBm9cFYTONUpHTjwBQ5IhBjVIbeee2dmCxdrS0cr6vA3EbWCpFNI5FCaUBbLwCGutN9ENb6QneOKzf%2FMg3KzQdpHK3Rcf3ldhN05wsXtDTCnK0izUzVHC%2BIXc5ZhEefdtvM%2FitWatyqtShLPHeiq814Ok8BTjJnFPnSYHvEBorR4AMF2&X-Amz-Signature=f71401df2a97b5274474fe903466da3d77547defb82a85e3706c5314a9e72063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

