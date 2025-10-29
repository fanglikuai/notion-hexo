---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X67OZNOK%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDYZYVfKXU8AbZJYoNyycoRp0%2BAokCAPL4zwQQhIi%2FjMQIhANx0ynt7CXx6r%2FdLAJzvTupbWp2weF1k%2FXK%2F7q9YPZOTKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzxeBTAGy7K7HYxsooq3AO6ilmqs%2Bif6xt%2B2hYlInbNSJ0fQD%2FcMLS9smQNS9EMdvkLftDYc47lZjmWrsg5xxbs7MjAcWszEE4rveAWOiG6y4vC%2BI0pFZEpV4iZlEoYsowyP1GwcPcW%2FO4hnEme70IfE4WFGSipcImZXhxJHr%2BgXZ68fWtD6Z5ZZleJ0Oj4KGjBhLENZG26ZciE6jBSPiR034VGR7N2Fy8M%2FnREg8NeEVSe1iLr9ilgk2T7b%2BK1f%2FsLz6Ku5Zb2SoI%2FZlBhVy%2FoxQJC%2B6D6LQkre%2Fy3BUofuwrjIw9peBvf4gPovAM6r8oU2cbkOdl5N8JOsUADOFK3k3KUgafdOwt5KmGSQawt6HB35mEzQGXuqwqLdo2vxsXlFrfhGoy40%2BbUED6u3VRGx7QwY8R0o2y6TP5i3j%2BbIln1URwFL9uADIol2rb4aeknWtf49lfREfuoE2tjYIHQ691sJQrh%2Ff6i6OSEQ2LizvkspOwM4EPKtU93HLpp3fMxT%2FL2%2Bunakzydig75ZQiApLSARRI5C4Cseudk8cVXIqmLlezusEwqycFPCZuIDenR%2F6M7wIyIstWjknd6ks6cuD0BVZq6u6uzLwSrf%2B0JUFWjLqNWQSu3y%2F14EdmxsfjHA8NnaT4RemnETTChk4XIBjqkAeyCh4AE%2FYTLVeTg%2BHwSalDY%2FabKuIRUquNfLMuDDUQyPzOmIFleXkS7ae4vugxiXGH4dIj6rIvf9pYMrC0bWJNtIFOKpfFvpPh9cSXzu11zPx5hJIfflcw%2FhuEWwPBmESBPdaFsAdblGDa5BtxVv2LOKYj1%2BMwUVwC0bwx67CUTREc9aaS9bXaiPu58JMPQPZlZ2mVyff%2Beh%2BWVqX8gBPsunb8h&X-Amz-Signature=dc55d11a336dd3cb694b30086f9e8aae59156d131e70ce696d224a742b87077c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

