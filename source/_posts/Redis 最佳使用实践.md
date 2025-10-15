---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CRWALS7%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGuYHNg6%2FXJ2grbUlp6oACoyQpbx5PaUKf0To7LP1PC6AiEAra76JY%2FJxsxVz2UWYQvpN8qi3MoFyR8tccg8%2BUtryI4q%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMY0wLCwy47GZ8sA9yrcA8wJ%2FRaynnua244pjbJ3ZZNeLkWpuCL8kkwUwJ5O06rWTRRsUcJ6fbsXgJOH0hvMusr%2FQ1QcS53HPffUqVKeQJGcrvRwq23vMAs1ylWYrE9G6c7ezzLcLNFLmthC5ghnPVefjbFUNP3HMcTOANeOltl5oZJZ7RDRFK01SHBvTVCC9OB6e0LwD3nqR%2BppEIstSn9wejv%2Fhwdgmw6qi%2BjeDpMFDafWpJpeD3jpyfLWtTP0Ikoh9m246hiMDVZm9MU9yGiZXwe%2BKHO%2B%2Fe9H33wgLI%2Fb13crM7x46l1yu29mJf1xTeeqVJkBedVexjKcAhhdLXDnqw%2FSwWUOEfrpyPxMUYhV4%2F7sIvKFC7FnCCGGiMNAJ2FnFzXQLSCMB7efx1RifL5kQhwdCU4sIcxbUHWqXxav3QHTWL0ZnurFrN73Sawk8f9RV4ZqtCZnS3iaF90w3FSTEBizbrEUcXufrv3cA%2BGp7QX3NR7iUwUjptiuyilhbTlY8Hgh9YujxUFJu85VArGG%2F1qu4FfFN5nRt0NDArhqeYassLUxKC5s4Ek7GWTyzi3Yd8EbA2xOqvQgTlE056U0qWQ09M3aB5nfxzrJVXbjXsIOVvSwsSjNiZqJXjxe1YkGMMrK4DzX5PEPMMH3vccGOqUBPGd86evhXxMHnlr3T4qKNFW0oI9AFna10MRMjG5VJyT79jlahT5hxwNH7kpA2UAKxsdHZwSKs0xq7spDJmwWAzIeHO%2BkxBrAf9QZUuG7lzDp7WEYDLFbnJqRrvxSiUIfw1G1qS6EzNqP5y0zrjKWOONa2zf%2BxNHpffp2ZS1skTSuW5mRonMbAzaMQIgUFz%2FgpsaQfGwMUCksQrLkSn9K1gN2ty%2BV&X-Amz-Signature=0f553bfc4152da8b687ecaae024bf98377a5da88270cfabf15c8b3b26547b2db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

