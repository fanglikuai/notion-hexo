---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNVJCRH5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T010102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2HcaweTAijwL1ShJzw6DRM6Mm0iXYPK3LfQTi9uKVIAiA02yZsQHcHJxzUtZ3UfQOOatduHlT9kQMVx0yhGvf3Oyr%2FAwgiEAAaDDYzNzQyMzE4MzgwNSIM%2BtGsvurEC4x4RfMFKtwDhOOB1sQorNIMdchsFxojYqWPzi%2Bfq0QTuLRK8Ml8FplQmAOZv3iRp1OrOAKBIgvTQOdDq6hF2jxG%2Bgr9t1rqoWakGgwznh%2F6ycMEQovfrv21%2FiLhq2uw53isjoAjCXyfikhbIMMemlw1cG1U0zTLheKBnaTIGFPqt00fv5GFyKs4w1f6Tp1yQyP%2F4X7GVKxCW%2FNpLZHLRspPceIUp7tgpPcDSbDJhR7rQe9UC7B1J%2FqGYZVVo4TP%2BHb0wYNu8efWTA%2BF8J9BkSDNCkhSeljsIWH8H5UUVdmXxAuzITocRrTeMu43ZlEWK5JYXyeQNVuBbwKr4u39KjGPyI5jkfjHDzTmg5BgxViExsTW2n6tllmL1Wzy7d33aK%2FLkoKFsb0xJQoc7ADkZprGpq97dinm%2ByqBl%2FjaGKbxPYe7NIbUpfEirakCR%2BbR9yiaks1h%2FZME%2BHEdPrx5lE%2FGg%2Fs7%2BTSWsY%2BkOE7i2cin9cLGV%2BML3kIk6i3G6UcnSBZi0K%2FCEroJPQU%2Fe10xhaQCyKeYaX7t53%2BlEquxZefBQNu0Vqa3NQ9gFj8BLVnk7nOd4fj1etRQ9e%2FQpa9vPdYwvSDaMz7ftrYj8o%2BRJ9Vil0IuENLLCIh24wbxH2wGS3OPivEwn6%2FCxgY6pgFER39B2dHiT%2FArKxc4%2BNILMHGU8DYHUO3vT5ypO18A%2BeAf11NRouc6WeF%2Bjf8eEvu5aXSfIS1YXN%2BQ4Ym5rKZHVSp6KqS%2FtvvM3%2FRSzyfGoL9lKxgk5Gi%2BS6SqiPXJZ9rO3R%2FcFM%2BBE0SwUIw70oaxi38CSGQ%2BbegjudvD64LwimJtQoV%2FbOEA2BDjaT9jQJowFUCbj36SOT3Ad4GTcw2JjTnqr3yQ&X-Amz-Signature=cd26770f0beddf6e5b10d024ca629eb30ed863181be8861cfb9960f8d8979a65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

