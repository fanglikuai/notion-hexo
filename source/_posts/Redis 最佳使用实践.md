---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMWZ7ILF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBj9p5U87gMsojIKXaWLpjbqNTc7aBDNbKvcABcfP6uwIgHstwa6f13g6fgZQffa07j4UFJnE1rk%2Bj9fIc%2B9%2BvgwQq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDAx%2BZBi9vmC%2Bd%2BlXeSrcA%2FH07rGW9pLmciF9mPFa6uDmaTmTxjZexrxLDAE5bURJmGIfIBX62TMsMTvHyQb7jzPwIv2tIKDKTsZnHZL20GjmGCcSSznq46dHrnuNeXYH5QQgzI8HBLTo6uzheuRX7JCWvh56IS81Igdq%2Bgb%2BXsJYCPDUlp2ZpLtDmUuJmaOJwm%2BuKYEzCfTbZDAOYoXN9vtYjhrhLdczoTbpiGrUdETIXlf94YQl9MEkKzU1Cjd520QbQj%2BxheKEqDEyLBaj0hv4LKkxN%2BdM%2FfTnZ8i9TWNIPwDtsHQPj7iy06V046qz6Es9wL%2B1lnWUfnrA1y%2BF4f6d0oHGIEF1ZOk6PHRGGJLlJMJbTwh0%2Bzjg1n16qYU5sxDc%2BhlMUEh9R7KeZsPxKbllZPxI%2BKh40Y%2FtGRDZ07PFwlncDJf4wRRy%2B6j%2BmHIGnVWaA3vVIO8d0y9%2FWx0%2FhcVP6S4OfoHU24P7Q5qkBqcIg%2FOVmzIBAFBkYO8qx5i8fEtiU0bylxmY8iEzHVS%2B0znHLUs3nBHB3fUj%2B%2B31R3xZhB4GKN3U1dLfc%2F9VsR5WJx9qjgveX8K359pkgqKWlOYDnINfdQQUBuwc3HNAsyoy0OS6KcNLkq9ZgoTPVmLsH5paV5wO6I6idC5XMJuv3cgGOqUBIp3N0rn0G0jVDLKFQep%2FXO4fIx1Hl9K9hMYDPZrf%2BP56iIQHKBjMkKF6fomd2VyFZylL2uYgxbhhhkESWgq7HzQJ4KOB02eaJOfkQLhoAWOqrmssHHBXQYoJVqq491RIpIuYwYgxd0I%2F%2F7HKISSJsx3Aba2nq7%2FbMAtsRup6EaPGRtgr9WMmDTdW51qhJ%2BTGURopdgD0xB9NGvEsr0K6RBstJwIA&X-Amz-Signature=aff3adee831b7418509cc943bdcce56c1fbf9fb03c99ddae475ae9397b9cbdac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

