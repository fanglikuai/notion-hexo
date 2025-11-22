---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLQDJLI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIDuj2KcDW4U3G9%2FBZ%2BhlVYJ3aPgSejf1VFpp1H1slEarAiEA3A%2B91E%2BPM%2Bl614iCKRopdmr%2Bhj3r5JAde%2BVajMXpDZMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDAV3%2F37px%2Fyx6qnpvCrcA6zINM1%2F8Nl9wvtDRR%2BNeJ8WdwMA8io9B5mjIVqIa4cD6AeR5YjOxKluac3y9XuYYlYhz9Roh%2FfTW0ocWkV1xnx1aMBCVriuPsARjZUgk9EnEd7jSuwUkQSjKsaWGADy4qo%2FNMsXtzFiKErcjoevPi%2BDhqyVsoT2Wl6DN1nCWGf5Up%2FfI6uEvGJaZB3kP3Qg7Zyz7aIIUq1ICW9hBnRPKT3ibXEWOyfwAwCE6tIEOx0W5STKF5NPjYGUWPN6NWDCjfGhAtoRAwAYTcq0%2BMWmR3sFMZKsY4xWFL5GXJMnuyZsRq9bflpfJA2u7x0rDn3%2FQgq9gZZ%2BWrIlaU8waOhSCqAvBLeM0h5H%2Ff39urIzr%2FqDJYkVApFMVlI6wO96Gol98fHMqyFvx5oGGis%2F4FhaEhN9ccQaQBf8xC3dYlPUuMHQ2vS0twYVPQISEDCherKkBJNme1ZDqZWqT4%2FxkroZGbmejqwS2g1yE0kCvuybNH3P2NNjHXl8GUrZKlRKc9BhsPQXvU93b4HA0xC%2BHXDPlgkRzCPAPfHf9WCILVhUmns4P1IOrfa8PCrpc22FgQlfv8%2FKS7EQDF7tNq%2BVzPseWaDGuTtwwD99zjUvsYjKy62YEDLZgLo1jKCpRk%2BsMP%2FFiMkGOqUBrtwrht0Jp8Je3hB2qiDYhusStsi2UAJ7VZV2lmSTeb1e%2BVMBOG62I3DBSTrRqXwgdZH%2FBQx6bIehluisBRy90blCTu9k62%2FoUOijRqq0AsyobwizhSilhG%2FS%2FSUJC%2B6f5BIbmoF3aH3gMT2jBtiQfvbZoKNlevS0CsHhofmsLasOA9Wr61ERhJcufL0I1sR7MXfKlNJW1157CJ8xGjy%2FaMWiQaxC&X-Amz-Signature=9ecd9022adc5fa7311590d74bfa3e34818fb4a19dd1c110f734ea6aee531dcd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

