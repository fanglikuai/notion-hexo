---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWORSES4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIDgo91qejHIPa5Im7Zoyf7BprWEpkpiB7mQTUj4uqZ9PAiEA4rfsOBiu9ZE9V87M%2BHaturNs8NNOfHaDzTdokk91n9oq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJoPVsYEbAn%2F37RAhircA2tp%2BjndzzvpYEV4aakZvx5eVtpC7x1QGBhA9udcUdXyW8J833J0jQ2UqPzj2USl8H40z0wAqNpZwgl1sG1VhW270q9DBPIXvG7kEbmKAJTW4FOD6TJck4Vnd3lAEKmiF8sQMKmV8h5uh8zgZUYjx9vGyanxixx1MU4FiswMoApca5WCCd99D9o4Nd9sTZql79ubmBMdVZF32L%2BXxSoFV85uBOUjKbyJicMhgbIGnhvoEk6t9Vio9OLiccAYxuJVrx%2FNEIdB%2BzVopihxzJHdKV8rZIEqPntbyKVyFG2v4lcOIHCEw5jvoUPZrmpwMqNqWjcv93F1rS26nK3uYjmhwLMPtXI%2FqL6K3UdTGey8M3mbD73Zg2ROQBCNwQNm6P%2F26X4FwgMKR6obYKe6ZdmJ5wmczZiMPXaQ1aVC3uAUSVNK%2BgS0YhzWPDkDhK%2Bbjz1tcVHbBn2ToaRs30eGdZ0xqtAiYn2Fa67jSZmC2F3DWiBDEJ3SoMwQqeFLxKZZgbY0WSZWQ%2BkpGwaaOaNigmGJch%2Bwo5u%2FnXUl6%2Fz6gttD54bz6I6rOX7NGTFsQYziZJBlRxg%2BPka8h5NSMCUPg70B4wbQfFLKf%2BrQ6zXwCkYRrbdsc16EIX5k8NpkOZs%2FMOTOmsgGOqUBb3dpnUdgN7Gl7fZ58b1RJ5aCDapxJs5dBUpwRQFHN0w%2Bq%2FM5DKlNWEic2gpncz33k8THLCMjGs3rkxml%2FONu5wBxQwaPtbYtSCeduxJCJ8fWxk4f38hHbLWNrtC6q0To4bEpMoz6z5wONDdBl%2BJ9lneVTL5FkkeDk0fgY3eeMMG2cowqUYExxUgzVNvI3QkYmnPIS8H2T5NSEL2gFFe3ydy6plBx&X-Amz-Signature=8e94043785dbfb8c3016f93ab246e4fccf7127e390710209ce6f19c8da65b906&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

