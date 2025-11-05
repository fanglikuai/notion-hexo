---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYHAUIW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3NNrKM%2BLXSmBXERpHP6z2MvenJCs2isbwCxJHk0jAJAIgUMfVgQUQSnmQ%2BXK6OlFl9aOpqyHyNKSAUx5W7rTUiOAq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDCYswQTq%2B%2FT1Ui%2FkACrcAzV6P2dxp3jVFTFVwuV%2FH3q0qbsPO0NvJuGmc9RHXs7nhh4936cExLbssaJPhM1%2BSMK6PVbBFgVCjpDdgw55tHpWbiDuAhv%2BhWhXH2M00D35qJpl1sqMFnbVqxlkjWN7eQHBJxKVx%2BOrXWnfMKDW%2F0bjsXw0FinboKeLSgi1pzMJqRzZx76zObNSOsYeW4aeOmq1xr6WAgfQ1iK8OvmMsSzB3X6rTjKa0jfW%2FSj4r7ocOnZUyLv%2B7sTFLLeMII5kCtd1mIOKvEVzQDzxcTJnVa5c5NBWrjhsrTKlJzPcK%2FD0CGRs9CTMNNwJ4PwFibmuAxOeIxTVAoaDYnB9Gp4Wh9TXTmHfSSP78jgDKqa2km%2BjkpB%2BhjsQYeorjGzLBcoW0BlvPjZZQHKR7FHGLZPx0uacQmyBvgU%2B9VLOQdZneqLm7Rs9t8vkHfTSQbmXuXgkowoMjmuxjXSUmSM14QpHQY4a7mqU3hZbKyveHwLT6wuPyohPAdvpRHxt03LOiYzO12qLeXPSnbphcv2uKY3NmvKX18MJ09OdCy7sZP8mX1g%2F9GH%2FsMopIefZoTV%2BlDzwOH10Bzebm%2BOTdp%2FZz4WASJHRJZnYLRqm8Wsdiz2N2pEH%2BYWfYT8J6fvNdH1wMOjnqcgGOqUBi%2BtNCBB9SRi5C1cdMc3gvGscIdY%2FrkjJLSFvv0WPeJ9u4YZwEFGEKsnLnuOu7%2BQTeQYn22x%2FXYgES%2FigR3H58ZKwWOTut5v32dOAmFvZ0gYL%2F5P78N6WyhTHZIjkLl6gD3IrNgn1OUTUnr4JdbaVPmKkZ6kRtYZNNaHFvjJUr0QRLzx7Ehwl0%2BYBK8HglMZFlXvMIu7rnbYA%2BBdroZWdLUMpgIhe&X-Amz-Signature=f467e4524366d5625872127ae74cb3874975cd4b5e73f4bb35a3f5dcf5be7b15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

