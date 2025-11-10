---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFSMYXL6%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIQC3CV3BU7bVGTuDi6RwAoGQfWnLdALt7N72m5kEv3hJgQIgVO7wWB6wKInPmDu2YXjtrgff%2BKp4cZoUZTk6NSQ9tiwq%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDGrowzDryHNnsKHeVSrcA%2Bajd%2Fw5tS3P4g%2FiMiBS5iKtC9nDi6%2FgsfJaIPg9joYXJW3xthIb%2FRFY%2B932Z58MhWUm29mkcLwObriAMpuxLMTJrZFkhxvX2WAbWQ%2BqeQtmeBF9s%2BWwOTthRLDFGPUbHw7Ot%2BjRxJsWSUoOF%2FXq9UDtVuVP9ljJFWqtdryy3pdjllYnZrPsi1pl9bZ%2BtqHMFTHmP1jhcGeKDg4R29X0yrjd8A3ce7cLABIbUZEdrsI9ObnUyz7oAbTsjIECDK5JOr5Vw7FxR1gjniyh1yfypw6ogsIqOISN7ihUdDa9VzWqoGZZn90yy4nPsPKrV4U0k5CSDke%2Bv1I8ub4gmjwhlR%2FYPdP3EKbG1UwkuDhC46IibS1dEg0t70a0rHqH7skTMSff91a%2B0I0fU4pQW5zRXbE%2FykR94tjg3QsEdaIRQq9Uw75S53MPey%2FxhEdepu8F30qEPtyk24TbrWvFb8zdnb6ALuAnzUhQp1liGtwmFUStexn1OX5Q6L1NJCO5OtNnmlW2T2SjSixWyIGNxnDgxcr4kKl%2Fu004bI%2BncBA6RBvIWzJ2UTE6wa3t4IYAlteTVb5Upne8dAyYEJdURVFdX5x2ZNP5NJgsSdrfT5mHq6Yw9sjJjKXtXW0PhIC6MI2Mx8gGOqUBRzbzC0iEmMOHAcu6w8Il%2FLmniA8jNkW%2FI4jX54oc9fR4Ujfiy57o3r9u%2F2Z41ufmyA1B9PeK71XON1fHpmgmkQFuJJ%2Fxq5dFi0ywh3hFX4GVskCyerVMR7HVuIaNAk8gN0zv0ctDFvOyAa3VVQIgBAGgL8kb%2FtRK1T4cDbGoOHkUcyZ0dkkuNOyB2TExf%2BqLH2pqWVisXlUGRdlp4R%2FE%2Bor1KsXF&X-Amz-Signature=af917f8320507913e4cb67e58846ad5b8f2b7312e8d3155046087adc2a3b540b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

