---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRIRXAZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIHbTkhMlj21EjTIJgd%2BUWbIuGCvakg41P47a%2BTlDh0S0AiEA%2FLvTRKrARyRS5RTHPvDGkC4%2F%2BT7TL7WdilIYu49iI0Uq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDLGz1NV0WtFRWnSAPCrcA07eh3%2BivmO8FFJr5ZFLPNdjbnjApypf9ckUSNAIwi7NPRsJiEmLPfku7bMUZXviYZNLNUmcX5cIZDWfpnOFgoLa%2BGK%2BmGyPGJFa1nR4pKGzJ%2Bn4LYulLvmtIs819zu0Zl0pCIyLBDyV%2FpFPhf0SjWpGcZbP7f4dkE5yw4HvwzHl3wf1LWTm5kCsamh2wYTRf9pH70QLhgxqL%2B8RKia1z3F5SGyr3JcIG1upva0VNQKxg1ysiDJ7UP%2FPYgUnG9srl6aYWXKzzX48t15qfoUMKOwhC6uZtHK837FkVzwaMJ0Qf0YPz8CRhD0U%2Bf4DlQlRVcTApn18SLQKLlu6RmozGm6Qe5dPMFJMTqpnkh4dP3qp%2B8PrRnbai1PPxVeRtOgPmPGVJMtUrxNEKosnibNih3CUKhqyvj3lNrw9la3r%2BRnej%2BneyFqlYWGGeJbxs%2FE5gOIy2OFdTUXM4PhmaILWmsKFLSLXHM2%2FDFUuIfp5UA%2BKnOQBb8BmTikbNTQ67GJIlcPMFH8yGG476xaEyu%2BYnOQiXHVTWU%2BB50zOVsSouL2m3xHM%2BN%2BejS0W6ERktoQfgGaj1k%2FLIcsBBTkavPS%2B6rZU0MzhLL73S8v20JI2kPvS7fCV0G0hJDC1hinwMIO7jMkGOqUB1fFW6LJkFvEi4KlM7xqihspeiRFiBVKO2jm5sCvMKCy1TdvbK6m00UxgBNtICswUR%2B9QJRfT1Cet2SOz23zHx8HIG8urQJWwM9fhoFJxm7pMnbE8ImODiosbPTZ6Im4M2nvmgeuuYwYn%2FbjA23dND2%2Bc%2BrOaQFZHn1htZmCQ1zepcSSfxcLmby5tW4SuLpSief8SZCSnQ4Xb4fgD%2F0n7QBBTAqFQ&X-Amz-Signature=bbd538c4138a40f2acbaed6df61d31350914a97edf58a27c97db050285e645cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

