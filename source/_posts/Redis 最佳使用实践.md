---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XWGGI4J6%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQDeFwztbmdDD5wr7yb2OtJEL31D7nBU5tWQ73cfqwH%2FxAIgdneeVfO909tVIp3z602gbpVH5Y8zXLfQb9PbfQkNdhUqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEDHektdT%2F59sIXb%2FyrcA5y2tqSqEataz3TZG4D0x8B4aGLI%2F94WAA3Ybz8Jz8UkWgPbtvicYsIJw3nEYqY9RZcOb2OOYx3YlasrrR0oi0LBlVNec0LF%2BXlK5ZJsbx%2BUDLJ9QW483oVgyEPv%2BQEBOjRuRiBO2S0TBSt3NEXvGX4qiBREgTlaNWp2Lr4VplIQWmGJoU%2BfAjFVx7co%2BNPDFbTH%2BJVe8VQP%2BtPxUJJroNqeKK3r4bwia%2BDuobBsYhY3Yz6hSd3pNNKMMPSsLQzVCNz7clGHZ9nl5HF8YTEWlt7hu4r%2BmWQGXiB1TuzseiXxmmpQfCsdZ4xDkATiPBIwHXOLZvl1aJFk8k%2Bu7m24fzJHL700hf%2F%2FtN0q2buSQzQS1YI%2FL8Bs9Mz%2BsB%2FMwSerQWgr5l%2BLNKfS%2FvhDIyGmil%2FEcouv%2BXn5ExxblawgtisqXafpvuQfVNngufurjqw6CZ1mRPw8hZYBppBoFqe8vpHA%2FWr2ilgBQqYcrlEaufrYQHT8UOKc0RZv3jJgxSmvh6zx9bgCTrhgB%2BeutRrEbrVXPTAPFHrWWi6BlNjOqImXgUhRuYXtLxRFMYbmBLrFCZ3JMTiZQVtzkUroFHedL%2F8ElaWMomzKCmIq8vPwbrdRCVPb9lpW5FMr%2Fd0uMNTn4MYGOqUBrAK4iCt6W5jWI2oRDrZVm6cYvkzEZtQ40WFwK%2FE6yZrYxUMU4dugD%2F8D8oXKMtP93RG6sOBVAOhkKzitEThzgkuaUgPPUXdrCb04I%2BhseLdRPCrbE8XYZCaBlJkeL7X3o%2F2XWixLrTu85oDU2lZcFbpOWf%2FjOxMO8Y6yU0AYOcGAv318MnA%2FqzD3KtxcQfPh3FIJGYYfrKjQWnN%2BlXP%2Bl3HzCYOT&X-Amz-Signature=bacc7e440c6ad5fcc370a6bdbaafba6303f1bb2e50461dea28c1a056e687b31b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

