---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T55NRDDI%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIQCuEqbPzlIyl%2F8N0eS5kH7A87owTKf7ZxyiLer2zaR9EgIgZLe5zpZ8iEV4hGPYON44VgkpRsP4QSaurwYrrEEt1ZkqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BBHV%2Fq7eBahkER2CrcAy9nLltRlb4uPmrEVR%2BOcEUk7UB0sJKXeQOWKR%2FFNe8BG5h0rxXfgtf7gUtjWq4Nv2HVxRKS3TKy%2FZYukdn2vzKcMmBAr5W5r0NUH2JdsddxopzRX3kkksoi2O0QQ17g8dqSwpfuTzwMy56AgRNr6QAeQBX5mlWpCOs29OVew9tQ1QWBaR%2BdegRbLmm0Eps1Ksy9yWfSRShdJfWpgGmdZeg5l%2FCt%2FFIHN%2B%2BPLH1zUi%2BPaBgcJqQ7FZE0sXd1wqzagXmA9o77dyOQGJJ%2BMegvnLm3A2JnJUfvAuJXUBuj%2FPAoNdsgjag1nm4aJaFy88iv7bdPEhIbCp1iAkr8A8f1HzBCX6%2FfYHH40jt2wD7hAmD9fv5KyP%2FEPR4Iqoa52xh%2B5HJ7Ag3%2BhEcA%2BFyAuJ%2BF7xBdhfSCOgV%2BfVhHHLiPypizRg3CXAajbGTGCvmZlYKmZje8ca%2BACo%2BNiRfQhpqn6YbDqdXYmqV2o41cvZhkroxU3uRpasAq46WoqWk2JsVCrm3cR3lSJZ0%2B0MgvdNt5Kja33cKbfJ6TelrkUS9t9RuX%2F%2BrcDOqxcYKkuZraqev3zBXW3VXLe%2FWrNdOQAoYgzZr7N1nMDS8GoicncdzoovUYoAWq5zdNi%2Bk%2BI6dMMMfIgsgGOqUBwTYL9Ay0kJP1J3n0%2F0aOXMtMftbjo9QmdFTbQNQB8QXDLHUo3sknDZIsUJNq4PjGAETgvCkolRroEIOFtFFLyCsTN9UjxygkDIlDWWJTaw0eKRaii1Wm5CHyE41QL7%2FfGdWP%2FtDztCgFJv4E52Oa2VkCeQk0popfQVZ2t5mWrY1h%2FqUvCIg%2F%2Bj0tRD%2BnVOoOwUeIx%2FvorkrGoH%2F3TFXL4Qpso77w&X-Amz-Signature=f9f5ff0445ad8c1234ce1f17d202142fe2c960e0fc6a1379dbb2332a6a058654&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

