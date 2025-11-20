---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WQVVCHI%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIDUul6by0v3AQKSzJfzU83HVWx%2FiYPfN4MXorDtTTIAAAiEA8dOXgCFyjpn0Sjxr3Pdd6A%2FHsp4c1yIVE9y9kDMlhZAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ3%2BnzUUsdPeiKtttCrcA7fufBYN9NR6nuuqwIeWkP%2FfJp%2FRlWulqcpq8F5fnmycvJ%2BnHd3UbHW5%2F%2F8OjrazAkBeSzzuc4s%2By0fijhqOrPqBVoNOcmW%2BxhVOBwKiUSZC8J1xhhuqBYwxfA4g%2BPAnnyi2h5tz35hBzZNhYlcgaDQpjGxGOpDOdnMFJOGaiL78265uCfmwSSZdIzEnoAWVFP6ZNRiQCtfMJQjWPl1g1DOxyeDH4XGVBjXiB8Z05XDkLHclJcFia4liydAc8W18T1nxZcrDw%2B3O5lHoPI38YirvJqpxLZijI4gadEthGe2qBcfKi%2BwoEHgxV9xtF%2F3qe3OpWPgCfTb2Hx0FPR08ijPRV%2BQj8whdvi7mJGjoU2nBI8bkNhcdMYHmpz1nnuFG2hChoOcHU208%2BI1j0eiL%2FtEM4IeOVyu%2F5qg%2FlWAQ3zNGuFDB3RdwzKXr2t9T8gfggXR%2FJq%2FxKepBlRr0WXxxolkGCl4exSQjeK9o%2BaCa%2BrGB6G5%2FbKury5dTeLIfsSM3d5dCSyvd8eM9foh4q1NRJp%2BxgDVlJ5Cddsp2SUxIyP8biGtGG0oS1%2BTsA1G%2BgQQq2yUP6Ea%2B0OmJVRs11%2FIRStcNzMnh0Vx%2FF3vTqaNX1xybZFM3P1nrCrYDVIXYMJ32%2BsgGOqUBuzp%2BtnVLWX1Vy4skyljww2L7KXWivYyrRGl%2F1xY8sWm%2FYaBRtBozxTEWlCsvSB6dmv%2Bd87ws3TuZhERnSDZetp2xHBP70VvA13zK0z5Oj2e9ORilKOccnDzt9XpRNzD8N5mqN1dX%2FYLQGv7JDuJz%2FkbNwQaHYmGRprqicjpoxmGxCzuzMQ1E4jmiRLhKeTSarm8TZRLDmU3ANQp1MaxVD%2FYGFevs&X-Amz-Signature=539e774775796cbb74fdfad0792d41074c3b7e6d03ed4537eeb97b37663535b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

