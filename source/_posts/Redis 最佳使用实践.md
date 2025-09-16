---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEFOFLRB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIEqYRuATGrcxBkAyJxqCqcTGaaWD5wll1Pg5DVGXUas3AiEAwjIJgwKvvIixS1%2FyeyR%2FjOlQXw2ydY37kDkwgmwvtEIqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD%2FDy67ostCRY7NDhSrcA1QmjaDfoTimnhf89dsjyibAWe3EAC1V3QGO%2FawmMuO4PZG4G2rM5dXjq8AFa9qzZEkWYGTxyproWeyhT3k2fCBR9XCqt6VgxQ7EOH2fbtJv%2FVB9oU38lF6RCIQa7DukLnIM%2Biu1m4Mmrra6wJs%2FMkHs2rsnhM4g%2FpOuZmI5eWk7%2BMXFcS5KSdo%2F65Klw6gTZ0Q5SPZKBYQhKyXXEbcvEjM5XhQRY1pV6EJCj8npFcfdsSzHJ%2FRZAlA9xTEqtDWansryEKKOjieKWl1%2BjXIacKwLVQUMQa%2FaFz%2F427qVl9do2AdC273tq6zYRofJeE45Ymo36sVFvXW7nkGNeRbmW84TK5LPNVXEq37gKyvYAsn3tnWlC115KZkqA5PudkOGADA%2FqCFcRO0T3VdLADm1mnp7to8Oegz2uvM4Nzky5KOKr1B7Fuzni7yl5X9%2Fq5aASn42HetHZqPwdLI70KhGE5Fn2H%2FIb3McbwKLRrBTWphiruXcenVmqj95ikeCUHW4bcvXk886Ur7cqfkUM3Um58Xs69ei%2Fre6lmbNHPHm20RX%2BeKLgJ3qNpNtt3V6wuV1TcIUAXNsEkPJ3%2FboCr9aDx%2F98%2BAq9pPyxpkBnqMAJHd0kpZWqYRVjBk47O7QMOWCp8YGOqUB2VCk5wn04HzyfEq5WDU0E4TnvbYiY8%2Bn2JW4zO3NzSWLYkq57saDI1WidZJq%2FSQnKSPCK5RKdQBt5p1bEWbFJHkrh5Lkdy2MVYvNBXSozbmcanERK%2BGO1nj4RR1fc0eIO6TMYY7tedD6BtJHNLHnmzhNWDYWYVuPa%2BVX4y%2FLyFFsCvl6RRDaRF0NDemOpL5bN3XqnFtCx0ek0yfblfQL81R71kdK&X-Amz-Signature=4a49f7201b3b5154a99d8c8e331df922181c47621a3cd5fdfbacc9722aabb76c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

