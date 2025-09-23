---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPPTK5YM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoJhT9n0yx%2BhcSjJePQ2yyS6233m%2FLywg0qT2yz0ctrQIgf2lPDFybb9yTXFuyAJ9eG5%2B%2F1O8jqJ9OaUpd1VEoYjMq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDDMRamQE2iiM1Yk3vSrcA%2BldcexHcvAd8SG0oVKPugDcnsieM9OB24QZfKTVE8CoMXb8brCB3D9Nm1OEDuUauapdr2JS3G9K1x%2FxkiXba7YWc9xHpIcnlOWbHmsPkQO2DDKWCbHEVs7yWhp9EBmbF2Ryxyx6Pq9gZoo1CsBbyhf88JGonYsC9rD1DuJLlQEtbWsIpsHo5exLmoITq5eQ%2F215Df3KtdZAbD0DrLUN0p76pgZGpaemA551WWiZTU1gG1bGl9tKWrGMEWYZJ4wQjccrc3u%2Fmhl2lXRb4X0kzZXsD0KpakkGcSwvRAj1txVOkQje80lAkjsacqfu7ECGO3z%2BVtksNthQLEmD4PDSSULgU%2BQRbYddynDteSw4MvI0SSEdrkS1uzhsma5n6CR07SYwo0xTN2SiMqachdll%2F3vFWzkJvDfHKylx9p6N7zlnFSGDiXKdP7JM7kYJgU7%2B6%2FFs7cc7wj7ltE%2FNSaK4P5A%2FAcxlq7g%2F1RVUHID0UXGdqDva71tQrXqtNXfXcSRzcxTRuBFLESDPSUvezpHk3s0anGJWtKKsAr8l6rrOz8XhtT0RRXb8%2BXLc4vA5fckC8Uh0Z6poy%2FzgCJimD8jXpexB8MUzYMcjgy6sOMAdiTA%2B%2BJ0ecB3j6YgYuq5VMJ6by8YGOqUBa2fy%2B%2FHs%2Bj421RMkAZ9WR41nHCFB4Jvmy%2F1o1%2F%2F4C0FbIid5HkLmEwunrvedANKHEuYQQZhHvMNXsvBb115Ba%2BMcEaH%2FYnlz7IdcVUZZCMwRSCN6Z4JkvKJcFsc5a7sTQRXEcoIISaXFT3RcaGxv9H%2FNOHy62QEIhcRbz%2BVLzTzSrxthBG1ir8MiRRkVYuDaqsJ9xkiRK7Gp9k%2F5m2bjvlYbemOX&X-Amz-Signature=10c102b326995bb9cfbd49b4a091de695041ab96bd315f4e3b9bb381ee92e69b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

