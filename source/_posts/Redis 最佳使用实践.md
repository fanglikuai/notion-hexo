---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LQGKGOF%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCkN%2FtN3quJVEde2ZSCl7F0UtRvwdpWxlxx5Ga1lGMoJQIhAP%2F56DJ3iB8j74vVHRuLsqJWa5za9WkCRy9J7YIk3fN1Kv8DCDMQABoMNjM3NDIzMTgzODA1IgyTV6lspbPNOgLzhtoq3ANNbdYMyCWP56g2K8nGwxT4AjrNbOEqgMgBZRRfbYdi%2FOIMdqmlVbcGWnzPMDu3l8TZ9wMuX4GYD7DhLovZ%2F%2F2Xe9mdb6GFd7S5EcdxFv7KmuxYOvtxo9cfE%2FDQhdVrqY2jhrU1i%2BduVLYjFxM6h6gwIdUdQJLKEc%2Bgy894G0GsN1ZIZX0VwctSkJeZbtKbuANbkkgxRcEsLZOsMeJjvRS0XE3BIaxq%2FNQiA%2F7i%2BnsP0hhwB20O8C8%2FPY7E4k7BUdXBnonVwK2MI3Q25g0nnZerv2cBGcS1ZFJIfNetCx9HbjzbIq09U8%2FBKbUJQ016YBAXPRPzuq%2BlgQBmDXHZvev3qunOSMhKCTBsEoyYOczm4dC9hJwVgLh%2Fn7t9hp45uQqJY4xyZIQIXSfnMzUquNFI9rSg2%2BE8biHyvP%2BqPraBOgiTqetJFAcbGE4shyynEVSfX5SxLHmCOaQdEWAohvNdODSWYEIa4d4PPyJ3BkEQaKwwYOwHN0eRjYyLe3aNkY5ey5nnI3aCRFQaiJSn9Gl21MiWot6R8IDc8dhJ7z1s6kMmQiFJvcnSqUmAQGwRihEbh308hbEit0Q4zrJdZXT9Ix%2F6fr06bFGX7IcLVUrjAT33Atael0oGhR2QijDHt%2BTHBjqkAb3zqAOlVq1fItHGRSGM1f6xDnX5JrAO0biapnDZqqVTB%2BI1AJFAoo0LyEIJ1bj68BhDbd%2BW%2FVYxZdBxO2AVJmZvHyK2ksnYn8Q1l7bOs4kzjU1oWwbqkW5E7rI8Nne%2FJbZbNDiYuXApknWHCi6aWeE7ybtUBoUAPv2UuKc%2FOpQcUhcB8eAOnMRmZVXEvNnZ6Kboq5v4hXe2skAzg73XuB4wYqmG&X-Amz-Signature=dae1c59281c28aeecb2fa82e60c8dffd62214f9fc79d47b4bb160d45cc95ba0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

