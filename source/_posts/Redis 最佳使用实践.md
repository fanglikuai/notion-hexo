---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PGRT5I7%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH6y0JYeE2wIxIFUwk1c9ktqse9y%2BE1woxrDSYRbu6JmAiEAhYAWgA7uUMMFJ6viZThowpJF8sr8hE8YOCgOjBFX%2FNIq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDEdJKeE31hj0NfOsfSrcAyQVhlkJlmAmOM%2BHgx%2BqAAuUeR4BlQI%2F9KJ6qqnBmQJOYPdCZQx9cN3v5QO7DFj22kMThi4GsL%2BTUgzxFj4XvnLK5oZLgnt22LKhapFvYsNRC%2BwbOIgvWyGM8bW9sTJtYRuNMeeVSTkj9HtZil1JxYbR34KFpSOAbL6gevEgTQp6cp7umH6Kvz2G2rEQYNSnZOpBBNP2nEZBH2S0K%2FWlsKhzxm9l615GvuVgr%2FUJ2VkJ58F9iMEJRfAARwxSJbnfPgUYBEat%2BvknwrvhC3PiSUwAnH77ITr3a3TsH72ZH0uegUpbyiWnV4YojlYbpekAjXx%2BDzE7fe3Wbg9AmYXpuD7KhiZQO2IXu1SnKa7S2xMKcqKT41rnECt2NUhbWaryStG%2BShhs0Xkmxb7jL6bEUoCkVMlJzYT2X9Tb16RqGrpcXd%2B7boqiq0UAm4StffrS5so2J9%2B4QQcJ15yqadahwBvAwoH1HcB2w5pKPHiGFtl3ZtQZjNNyePUhL88bfAs0lmL8xWO1i%2Fsl1lyLL8FtBiouMCx9I2a3Nmar4Q%2BxIKEjar%2BZrP8oYkwe9wWUDICmk2Wpy6ZPhBr%2BqCDrn%2BoCpjgJEpTo6l83%2F7WAd2TnO5JJH5dV0VdLUtRUGB9NMKfhg8cGOqUBweaSpVrynROPZ%2BcXkITv5f10r6YS4I44bigaFtS2hQHPXPxMH5n94xIpWFdnU1eaQHUe7SOLn89J5etyoGHDDTriohRBnQyPcKxSTspr%2Bc4BF8dZfZHHlHl9rZPsSj43u04J%2BeNaKbRkrB8JR1wUhAExgOAqp0TuACsxfDZnJBUyg2JrjBfimU0SyK%2FCAgBGd4OR5aQgCvfvTNALxoF0tVkvJtYm&X-Amz-Signature=bf339b848f3e8e3810d6a3c7e4ef30a44e64f2c301e8ae9841937c5d780948ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

