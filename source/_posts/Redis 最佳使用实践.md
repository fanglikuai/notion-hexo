---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J2XJNVH%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOjhz7%2FV%2FxtZFkWFYJ8Cy4DRuFz8bxJp4ChJH6NCVmAiAWu5rYpieoI3ILVVvxY%2Bgg%2FYKeATKo2x4aZyOFZ6b9lCr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMCGCKzuDzeYEWFeGqKtwDiXe6KAMX75GwgKHqWHYceEnwJav4R1VY1nP6XSHThsM3izh21QoLilcx8DgL9%2BVdZmlxfdgVwN%2FUoTy%2B9WdZPbKXOVFkf6tmfTGOYA%2BJCUCq4Rql%2FZ%2BIwKvg42vE9y%2BKG8b41UECFAlx9w7bWaXF%2F7xov7s9vMwyn1BPvzqjDW%2F68x0o3dMFjD5mhUJ4sM88jzysl6xSypKo%2FvVtyK5izopNC9O19eehsZOIdHWM0mmCJNrueaHGK%2FtQkB%2BYUX5DTW4C4cw4uvtB40xURSsTeyUV0EK%2FoWbfC%2FTJXg7Y5fAYrFaiPm1RvCtr4MlXjsex3MLhNO8%2B99Ag02wLxY01HHqpNTGNdZMfQ4TOcNZkiyu4nKeD2%2F5wfqxZ1NjNu4uNS7OKh3JbBZisx78ridZkecuEgWHp8D6f3FMQ2a5hrsBHh0wUGoJh96%2FvNoatxAoutjmaQ0evNqut6UoNaW1a98%2FjFgrrbrIrzINQj7k8PWAZ8HZ15%2FmCIvIcwHADF8s5s4UlGlmp2XrzlQ79hWQDf7HILPIAlnL4LbmeocYq3VGDIGybAKdgZIn2emwvYrugYdCshANnh6mUPvnHwhVY4mmsiTv%2FsyvvJQUvnQC3gDjUDUt%2BYl%2BQx0cKYZ8wj5LIxgY6pgG1Qd5Wi%2BA1wVSqcdk2w4NmqkUaw97aKrMcbVDb9OnT3H6i1sdtDHFhsJNf7WqdfFdWbbUXk2bOdUBl0vPGQ6BMlxS6y3oVG%2FkB2tShk1n5oj4pTTX9aoXAwagXoRNa7ktmh9jl0BGzgtvSQlgqSkDjFia3nrzqzmtMe%2FqHBeEotyz9TXvY4PcQUoOHlMZtZi2QMH2ea0iTpeccnNNZXovsJIZ53z2h&X-Amz-Signature=b865d616b7ed33143563fcd5b63e1a8c237228ba9c4eb489678c476d279a2f40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

