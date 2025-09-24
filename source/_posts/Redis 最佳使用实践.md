---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JKODN25%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBn%2FYEqPVTNScM7nL1Oty0BfmdxQxEzLQd47MTrsq2HzAiEA44GcTeP79QmkOBBHdFrxVL4yZfmXssNXvKJ1Ni51W74q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDGUJlM13MfwfoDzU1SrcA3rVo1CedfQlUh%2BKgGH9VkFXx32xWt7T0p72BB1k5wUgiwHuVsLmYgl3PPYgmyy77vH6%2F7O6IrNGbz8KAmAk03tGWTxxFnCAFY7oe%2FIeEYK3LDEuzTHb425FauDKIzqvHlDS2iv4n8e9y2w7C35XumMa8%2FnjKEsw5Dxf%2BQi5GwSKR6s9KOJkOqvlxHErf30HXPJzRvGtyU1xjNtC3LXvx%2BAmChyjoOofar5DaYFnrTad1k7HcMt3qDThIasMy803G%2FM%2FnYOl8G6cyp%2BTObQsD32wk30sNteT4Iajabr2cBHqgQqnLz%2BtlLWBYWj21EyNX6hDCAtT94nvuh3ikDliam2%2BD%2FAFJUzGtqk%2FO5ghLUG4fkc%2FNh%2B5m49QdTY99rmtDQrHeqMQe%2BwSLYgp0%2F5ojQwPfTVfXpwmDhnqm8qaAZDRR8j7m%2FO9i2neJEzrLLC9iHq3yivWX4mYM%2FvvZDnwKdaNvqyVRUIUh1haUy0nTWrVb8HsrRI1uZWO9XSJU0tDgezm5q%2FGseON%2BlWSMa2zg3w3H4oG3tfK4wMjf8XSDGOYV30v39aLh2hJMCOSxQCnKVHu36fzZyeIn7Hr9mHd3NQTxWB3twR9%2Fwcb2nU%2Faj5eJgA07VMcTMAP6SehMLb5zsYGOqUBMeK%2B5w3O%2BEdhM5ZI1IlWTStYlSxx0npxZ3dQepGZpggicS2K3H7hea8Myi4xFGxxIfMr3QoaVKtzKWUy3YriK0kDEZXeganBLpQzj3Nv25E9o2KxlhlMdptBKSa1OCtwy8vNyh9zHIRyqAX1h3a3Q%2F%2BxSZ9FB268hjfkI%2Fn6TXLVRKGuki8MUuYbVBPyVlKpyJSrdYVp%2B992Ma5pqvKDRvbJpfh7&X-Amz-Signature=f34d6278407f6f01cac250975c48775dcf2f79bb84e3deb76975a20301cafba8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

