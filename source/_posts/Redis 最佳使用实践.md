---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMCMLX6C%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQDQS3anK24sX2RS%2FmDvfjYmxNkcupoI2tUozhR519iLAQIhAMHmE111hc62R1gxe8YsnxyGJMDFnRQd9Esd8hbjBsVqKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRE9R82%2BMHFvsJKaYq3ANt5D7VTloueVM%2FXA72aA0nnVjBCDtXBEvjrkjw881GKFAcYhiAeGlOZLD6Wboo3QNqOCqiIXgW3iFdAGB88Etc2a3MjWV%2FYheMhQMhGGVqA%2B7a1Va1sWNtqEDlSe8IHe7IRaqsnaWYt0ZZ%2FZWOXlPmvMimkdK7WdI2YV2LtZfgErHBCGdCkc8JwaIcdxcH2jkwXbR9%2BoP7WKgRUzEZM0Mpwu2lXu%2BIXGkdZ1RHy1A%2FsuSLyYEVMd73Tj67hOP%2BoTh%2BhmVGqr2jHbVjK%2BBae%2BWogcLUWF1JK8LJAKJVuYg4YuhOT53eGYUbwFFnfdsYmp%2B4p%2BLnGqR9knRFusggXEcnOzza0O3D7%2BMept9%2FEyWjg0XJJDGJWwewxg2%2BoEHiJuf5iWceLq%2BAYGAbW099D%2BLME9Ok2PqgpjBqAyHiq4%2BiTwpgJan1gPbJN5HrXvZtLm1mhMq3UWa3FUnNYlXEHpYWSY2iXDJvRRD8shI25Aulz%2FXa0zRa%2FNasAWp1gAaOuso18VHm3yAU8l52QHt0iGK6%2FT18latVpfrf%2FCsZUS%2FIVezLyIIG65mEDh0NH8Wm%2B3NqNQFkdbv7TZdq1nwNAuu0%2FwrUnSGscAeQVxLPpdC1kFiM%2Bon6DUH5MpUAoDCPsqHHBjqkAUWkl4J0KNc2BzXzSIyqDhgHdVOOKdvwNpGuWvz4f7xZaoYO0llkftwiS4kRSzMRZ3LngBNaRWm4knzi8BeOIgvNwOLNwU12R4ue5Wib7FMd%2FjPsz20SKQfJ32hae%2FIIGVZmMxOzrE0btYw%2B%2Frj4aJzB5GpKVmLZSo%2F4h24LXyGZzlgpzLqeoLQD%2FfHoc68x2tFXVHlx0M7fbcYWOKOhGc7Vo8Qi&X-Amz-Signature=ef63440b608fa169dc19574cb8d95f78070d259f954931a41b592df6e8a314bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

